---
layout: post
title: Validations with TotallyLazy
categories: blog
---

[TotallyLazy](https://totallylazy.com/) is a library that gives you some tools to do functional programming on the JVM and particularly in Java, like: `Option/Maybe`, `Either`, persistent data structures, `Sequence` operations, etc. Unfortunately, even if quite useful and nice to use this library is also practically *undocumented* (and no, I don't consider raw code as *sufficient* documentation).

Since I've been using this library for some time now, in my pet projects and also using it in production, I'd like to share a thing or two that I've learned along the way. In particular, I'd like to show you how to do *validations* using the primitives offered by TotallyLazy.

## What does it means to validate?

TotallyLazy model a validation like a *function* on some data type that can either:

* Be successful.
* Be a failure with a list of error messages.

More precisely, [Validator<T>](https://github.com/bodar/totallylazy/blob/1.69/src/com/googlecode/totallylazy/validations/Validator.java) is a `Predicate<T>` with an additional method `ValidationResult validate(T instance)`. A [ValidationResult](https://github.com/bodar/totallylazy/blob/1.69/src/com/googlecode/totallylazy/validations/ValidationResult.java) is a type which can be successful or failed, and in the latter case can contain a list of error messages (as `String`s).

In Haskell we'd have something like this:

```haskell
data ValidationResult = Successful | Failed [String]
validate :: a -> ValidationResult
```

## Ok, how do I do that?

In this case, an example can be much more informative that a lot of words. Let's take for example the validation of an email.

An `Email` in this example is a classic POJO, that I express here as an Haskell record because it's much more concise and the point is not the structure of this class. The only peculiarity of this class is that *every field is optional*. 

```haskell
data Email = Email {fromAddress :: Maybe String,
	                toAddress :: Maybe String,
					subject :: Maybe String,
					body :: Maybe String,
					host :: Maybe String}
```

First of all, we want to write a `Validator` for checking to validate that an optional `String` field must contain a value:

```java
/**
 * Builds a {@link Validator} that checks that a given {@link Option} contains
 * a value.
 *
 * @param name Name of the datum
 * @return {@link Validator}
 */
private Validator<Option<?>> notEmpty(final String name) {
  return new Validator<Option<?>>() {
    @Override
    public ValidationResult validate(Option<?> x) {
      return matches(x)
		  ? ValidationResult.constructors.pass()
          : ValidationResult.constructors.failure("Parameter " + name
              + " missing");
    }

    @Override
    public boolean matches(Option<?> x) {
      return x.isDefined();
    }
  };
}
```

We've defined a creation method that builds and returns a `Validator<Option<?>>` that validates an [optional](https://github.com/bodar/totallylazy/blob/1.69/src/com/googlecode/totallylazy/Option.java) value by checking if it actually contains a value. Note that since `Validator` is also a `Predicate`, we define the corresponding `#matches()` method and use that in our `#validate()`.

Next, we need to define an ad-hoc validation for the host: it's optional (in fact ignored) if the system is configured to use a mock mailer. Let's assume that we a method that says if it's configured the mock mailer or not, and write this validation:

```java
/**
 * Builds a validator that checks that the host is present (unless the SMTP
 * server is a mock).
 *
 * @return {@link Validator}
 */
private Validator<Option<String>> validHost() {
  return new Validator<Option<String>>() {
    @Override
    public ValidationResult validate(Option<String> host) {
      return matches(host)
		  ? ValidationResult.constructors.pass()
          : ValidationResult.constructors.failure("Host is missing");
    }

    @Override
    public boolean matches(Option<String> host) {
      return host.isDefined() || mailServerIsAMock();
    }
  };
}
```

Finally, we can use this `Validator`s to "declaratively" validate an entire `Email` value as follows:

```java
public ValidationResult validateEmailParameters(Email email) {
  return sequence(notEmpty("from").validate(email.getFromAddress()),
                  notEmpty("to").validate(email.getToAddress()),
				  notEmpty("subject").validate(email.getSubject()),
				  notEmpty("body").validate(email.getBody()),
				  validHost().validate(email.getHost()))
	     .reduce(ValidationResult.functions.merge());
}
```

Note that `#notEmpty()` is defined in very general terms and can be reused in many more circumstances.

## And now?

This is just a quick overview on the validation primitives offered by TotallyLazy. A good next step would be looking at all the fine [combinators](https://github.com/bodar/totallylazy/blob/1.69/src/com/googlecode/totallylazy/validations/Validators.java) for `Validator`s.
