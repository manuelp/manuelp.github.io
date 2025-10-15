---
layout: post
title: Querying PostgreSQL with Haskell
tags: [Haskell, PostgreSQL, FP]
withtoc: yes
---

There are several options to interact with a relational databases using Haskell: drivers, DSLs, ORM-like mappers. For example:

* [HDBC](https://hackage.haskell.org/package/HDBC)
* [persistent](https://www.stackage.org/package/persistent)
* [squeal](https://github.com/morphismtech/squeal) ([announcement](https://www.morphism.tech/announcing-squeal/))
* [hasql](https://github.com/nikita-volkov/hasql)
* [Esqueleto](https://github.com/bitemyapp/esqueleto)
* [Opaleye](https://github.com/tomjaguarpaw/haskell-opaleye)
* [groundhog](https://www.schoolofhaskell.com/user/lykahb/groundhog)

Recently I needed to perform queries on a PostgreSQL database to automate some activities, and I used [postgresql-simple](https://www.stackage.org/package/postgresql-simple): a nice (albeit somewhat limited[^1]) way to do that that uses [libpq](https://www.postgresql.org/docs/9.1/static/libpq.html) under the hood. For further information, see its [tutorial](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html).

To use it, the `pg_config` program is required. You have to install the relevant postgresql-server-dev package (with Ubuntu Linux you can install everything with `sudo apt install postgresql-server-dev-all`).

## Simplest possible example

```haskell
{-# LANGUAGE OverloadedStrings #-}
module PSQLSimpleExample where

import Database.PostgreSQL.Simple

main :: IO ()
main = do
  conn <-
    connect
      defaultConnectInfo
      { connectHost = "localhost"
      , connectDatabase = "books"
      , connectUser = "someUser"
      , connectPassword = "somePassword"
      }
  mapM_ print =<< (query_ conn "SELECT 1 + 1" :: IO [Only Int])
```

Here we build a [`ConnectInfo`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#t:ConnectInfo) (a record) by overriding the [default](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#v:defaultConnectInfo) one so that we can connect to the database we want. Creating a `ConnectInfo` this way is safer because the shape of this type can change and our code won't break. Then we use the [`connect`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#v:connect) function to acquire an actual [`Connection`] that we can use to perform queries.

We use [`query_`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#v:query_) to ask PostgreSQL to calculate 1 + 1. The signature of this function is:

```haskell
query_ :: FromRow r => Connection -> Query -> IO [r] 
```

Given a `Connection` and a [`Query`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#t:Query)[^2], we get a list of results in an `IO` context. `query_` is polymorphic on the result type, which must have a [`FromRow`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#t:FromRow) instance available so that its values can be constructed from a sequence of fields. 

For basic types, postgresql-simple already [provides](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#g:11) the necessary conversions. That's why we are able to directly read `Int` values out of a query without having to define the relevant conversion. But we still had to:

* Explicitly specify the expected result type (`IO [Only Int]`), because since the query string is basically an FFI Haskell cannot infer it.
* Use the [`Only`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#t:Only) as a wrapper, which provides the needed `FromRow` instance.

Finally, we just print every single result to the console by monadically mapping `print` on the result. In this case, we'll just get a "2".

## Parameters substitution
This library supports [parameters substitution](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#g:3). In this section we are going to use it. But first, let's define the table we're going to use:

```sql
CREATE TABLE books (
  isbn TEXT PRIMARY KEY,
  title TEXT NOT NULL,
  authors TEXT NOT NULL
);
```

We want to query for book title by ISBN:

```haskell
query conn "SELECT title FROM books WHERE isbn=?" (Only "a" :: Only String) 
  :: IO [Only String]
```

Note that we used the [`query`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#v:query) function instead of `query_`, because we wanted to substitute parameters. Indeed, it has an additional argument to specify them:

```haskell
query :: (ToRow q, FromRow r) => Connection -> Query -> q -> IO [r] 
```

As you can see, there is an additional type parameter with the type constraint `ToRow q`. [`ToRow`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple.html#t:ToRow) is the dual typeclass of `FromRow` (unsurprisingly) and provides a way to translate a certain type to a collection of column values for the underlying PostgreSQL interface. As with `FromRow`, `Only` wraps our only `String` parameter so that it will be translated in the relevant database [`Action`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple-ToField.html#t:Action).

Another example:

```haskell
main :: IO ()
main = do
  conn <- connect defaultConnectInfo
  mapM_ print =<<
    (query
       conn
       "SELECT isbn,title,authors FROM books WHERE title like ? AND authors LIKE ?"
       ("Haskell" :: String, "Hutton" :: String) :: IO [(String, String, String)])
```

Here we have specified two parameters and the results with a tuple, since `ToRow` and `FromRow` instances are already defined for tuples up to ten types. We still had to explicitly qualify our literal parameter values as `String`s.

## Reading structured types
But we don't want to read just tuples out of PostgreSQL. Let's define a type:

```haskell
data Book = Book
  { isbn :: String
  , title :: String
  , authors :: String
  } deriving (Show)
```

We can read books back by defining the appropriate `FromRow` instance:

```haskell
import Database.PostgreSQL.Simple.FromRow

instance FromRow Book where
  fromRow = Book <$> field <*> field <*> field
```

And finally:

```haskell
main :: IO ()
main = do
  conn <- connect defaultConnectInfo
  mapM_ print =<<
    (query
       conn
       "SELECT isbn,title,authors FROM books WHERE title like ? AND authors LIKE ?"
       ("Haskell" :: String, "Hutton" :: String) :: IO [Book])
```

Piece of cake.

### Writing `FromRow` instances
How does all that `<$>` and `<*>` jazz works?

```haskell
data Book = Book
  { isbn :: String
  , title :: String
  , authors :: String
  } deriving (Show)

instance FromRow Book where
  fromRow = Book <$> field <*> field <*> field
```

First of all, [`field`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple-FromRow.html#v:field):

```haskell
field :: FromField a => RowParser a
```

[`RowParser`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple-FromRow.html#t:RowParser) simply represents a reader for the underlying PostgreSQL values, and produces values with Haskell types.

Let's recall the combinators signatures:

```haskell
(<$>) :: Functor f => (a -> b) -> f a -> f b
(<*>) :: f (a -> b) -> f a -> f b 
```

* [`<$>`](https://www.stackage.org/haddock/lts-9.13/base-4.9.1.0/Prelude.html#v:-60--36--62-) is an infix synonym of [`fmap`](https://www.stackage.org/haddock/lts-9.13/base-4.9.1.0/Prelude.html#v:fmap), a [functor](https://wiki.haskell.org/Typeclassopedia#Functor) method:
    ```
    (<$>) :: Functor f => (a -> b) -> f a -> f b
    -- Defined in ‘Data.Functor’
    infixl 4 <$>
    ```
* [`<*>`](https://www.stackage.org/haddock/lts-9.13/base-4.9.1.0/Prelude.html#v:-60--36--62-) is a method of [Applicative](https://wiki.haskell.org/Typeclassopedia#Applicative).
    ```
    class Functor f => Applicative (f :: * -> *) where
      ...
      (<*>) :: f (a -> b) -> f a -> f b
      ...
            -- Defined in ‘GHC.Base’
    infixl 4 <*>
    ```

As we can see, they are abstraction defined in pretty generic [algebraic structures](https://wiki.haskell.org/Typeclassopedia).

Lastly, we have the `Book` data constructor signature:

```haskell
Book :: String -> String -> String -> Book
```

Both operators are infix, associate to the left, and have the same priority. Therefore, we can parenthesize the original `fromRow` definition like this:

```haskell
(((Book <$> field) <*> field) <*> field)
```

Since functions in Haskell are curried, at the type level the `->` operator associates to the right. So we can parethesize the `Book` data constructor as follows:

```haskell
Book :: String -> (String -> (String -> Book))
```

By substituting it in the `<$>` operator application and the `field` type to its value:

```haskell
(String -> (String -> (String -> Book))) <$> (FromField a => RowParser a)
```

Now the type parameter of `RowParser a` is inferred thanks to the `<$>` application:

```haskell
(String -> (String -> (String -> Book))) <$> RowParser String
```

Now, applying `<$>` we get:

```haskell
RowParser (String -> (String -> Book))
```

Substituting this result in the original definition and the remaining `field` values with their type:

```haskell
((RowParser (String -> (String -> Book))
  <*> 
  (FromField a => RowParser a)) 
  <*> 
  (FromField a => RowParser a))
```

Applying the first `<*>`, given its `FromField` type parameter gets bound to `String`:

```haskell
RowParser (String -> (String -> Book)) <*> RowParser String
-- then:
RowParser (String -> Book)
```

The type-level expression thus becomes:

```haskell
RowParser (String -> Book) <*> (FromField a => RowParser a)
```

By applying the last `<*>` we get:

```haskell
RowParser Book
```

And since [`fromRow`](https://www.stackage.org/haddock/lts-9.13/postgresql-simple-0.5.3.0/Database-PostgreSQL-Simple-FromRow.html#v:fromRow) is defined as:

```haskell
class FromRow a where
    fromRow :: RowParser a 
```

We have our `FromRow Book` instance.

---

[^1]: Warning: postgresql-simple [doesn't support](https://blog.melding-monads.com/2011/12/30/announcing-postgresql-simple/) prepared statements, but on the other hand [Listen/Notify](https://www.postgresql.org/docs/9.1/static/sql-notify.html) is available.
[^2]: Since we use the `OverloadedStrings` language extension, we can specify the query as a `String` literal.