---
layout: post
title: Pluggable procedure calls
tags: [Haskell]
withtoc: yes
---

In today's post we're going to model and implement a generic and pluggable procedure calls subsystem in Haskell.

## What's a procedure?
We want our model to be as much *generic* as possible, so:

```haskell
import qualified Data.Map as M

type Procedure = (Parameters -> IO JsonString)
data Parameters =
  Parameters UserID
             (M.Map String String)
newtype JsonString =
  JsonString String
  deriving (Show)
newtype UserID =
  UserID String
  deriving (Show)
```

Basically, a procedure is just *a pure function that from a generic input stringly-typed data structure computes a JSON data structure as a result in an I/O context*. The input data structure is just a generic `(String, String)` mapping and a user ID (since we want to be able to perform authorizations checks when necessary).

## Pluggable registry
Every procedure must also have an *identity*, since we want to implement a *generic routing mechanism* to lookup and invoke them. So, every procedure is identified by a *path*:

```haskell
data ProcedureID =
  ProcedureID [String]
  deriving (Eq, Ord, Show)
```

By associating a `Procedure` to its `ProcedureID`, we can model a generic procedures register:

```haskell
data ProcedureRegister =
  ProcedureRegister (M.Map ProcedureID Procedure)
```

## Generic routing
With this primitives in place, the routing mechanism becomes quite easy to implement since it's just a register lookup combined with function application:

```haskell
execute ::
     ProcedureRegister -> ProcedureID -> Parameters -> Maybe (IO JsonString)
execute (ProcedureRegister r) id input = do
  procedure <- M.lookup id r
  return $ procedure input
```

## An example: string concatenation procedure
Let's try to implement a simple procedure to test our "infrastructure". The first step is, since this is a very generic infrastructure, *parameters "parsing"*:

```haskell
getParams :: M.Map String String -> Maybe (String, String)
getParams m = do
  x <- M.lookup "x" m
  y <- M.lookup "y" m
  return $ (x, y)
```

The actual procedure can be implemented as:

```haskell
concatProcedure :: Procedure
concatProcedure =
  \(Parameters _ m) ->
    case (getParams m) of
      Just (x, y) -> return $ JsonString (x ++ y)
      otherwise -> fail "Missing parameters!"
```

To test our procedure, for simplicity we'll just keep out the remote aspect of invocation and wire everything to a simple `main` function:

```haskell
register :: ProcedureRegister
register =
  ProcedureRegister $
  M.fromList [(ProcedureID ["demo", "concat"], concatProcedure)]

dummyInput :: Parameters
dummyInput =
  Parameters (UserID "demo") (M.fromList [("x", "Hello, "), ("y", "PPC!")])

main :: IO ()
main = do
  case res of
    Just action -> fmap show action >>= putStrLn
    Nothing -> putStrLn "Procedure not found!"
  where
    res = execute register (ProcedureID ["demo", "concat"]) dummyInput
```

The result:

```
*PPC> main
JsonString "Hello, PPC!"
```

If we change or remove parameter names, the function correctly fails:

``` haskell
dummyInput =
  Parameters (UserID "demo") (M.fromList [("a", "...")])
-- *PPC> main
-- *** Exception: user error (Missing parameters!)
```

It also fails if we request a different procedure:

```haskell
execute register (ProcedureID ["demo", "reverse"]) dummyInput
-- *PPC> main
-- Procedure not found!
```

## Source code

```haskell
module PPC where

import qualified Data.Map as M

type Procedure = (Parameters -> IO JsonString)
data Parameters =
  Parameters UserID
             (M.Map String String)
newtype JsonString =
  JsonString String
  deriving (Show)
newtype UserID =
  UserID String
  deriving (Show)

data ProcedureID =
  ProcedureID [String]
  deriving (Eq, Ord, Show)

data ProcedureRegister =
  ProcedureRegister (M.Map ProcedureID Procedure)

execute ::
     ProcedureRegister -> ProcedureID -> Parameters -> Maybe (IO JsonString)
execute (ProcedureRegister r) id input = do
  procedure <- M.lookup id r
  return $ procedure input

--
-- TEST
--

getParams :: M.Map String String -> Maybe (String, String)
getParams m = do
  x <- M.lookup "x" m
  y <- M.lookup "y" m
  return $ (x, y)

concatProcedure :: Procedure
concatProcedure =
  \(Parameters _ m) ->
    case (getParams m) of
      Just (x, y) -> return $ JsonString (x ++ y)
      otherwise -> fail "Missing parameters!"

register :: ProcedureRegister
register =
  ProcedureRegister $
  M.fromList [(ProcedureID ["demo", "concat"], concatProcedure)]

dummyInput :: Parameters
dummyInput =
  Parameters (UserID "demo") (M.fromList [("x", "Hello, "), ("y", "PPC!")])

main :: IO ()
main = do
  case res of
    Just action -> fmap show action >>= putStrLn
    Nothing -> putStrLn "Procedure not found!"
  where
    res = execute register (ProcedureID ["demo", "concat"]) dummyInput
```
