---
layout: post
title: Simple FSMs and event-sourcing in Haskell
tags: [Haskell, Event-Sourcing]
withtoc: yes
---

In this post I want to briefly show you one way to encode [finite-state machines](https://en.wikipedia.org/wiki/Finite-state_machine) (FSM) and use them with [event sourcing](https://docs.geteventstore.com/introduction/4.0.0/event-sourcing-basics/). For the purposes of this post I'll use Haskell with just basic data types and functions, so translating it in your language of choice should be relatively easy.

## The problem
This is the particular FSM we want to encode:

![](/images/FSM.png)

It's not very complicated isn't it? It represents the states of a resource (for example a document) and the possible transitions between them. In other words, the FSM represents a _process_ that the resource goes through. Potentially with multiple users involved.

You can think of the states transitions as _domain events_ in an [event sourced](https://docs.geteventstore.com/introduction/4.0.0/event-sourcing-basics/) application. From this point of view, the document state is a _projection_ of domain events that is computed by processing the events sequentially according to rules represented by the FSM. In other words, **domain events represents _what happened_ and the document state is _derived_ from them according to a FSM**. In this sense, the FSM gives an interpretation of what happened in the domain in the past according to a codified process. A nice property of this interpretation is that this process can change and the document state can be recalculated by replaying the events.

## Solution
First of all, we encode the possible states:

```haskell
data State
    = InProgress
    | Approved
    | Rejected
    | Deleted
    deriving (Eq,Show)
```

Nothing fancy here. It's just a [sum type](https://en.wikipedia.org/wiki/Tagged_union) (you can think of it as an enumeration).

We also encode the possibile state transitions (which are events in this example):

```haskell
data Event
    = Create
    | Approve
    | Reject
    | Delete
    deriving (Eq,Show)
```

> Please note that this is a simplistic definition where we assume that we're working with a single document. Supporting arbitrary resources is not that difficult and I leave it as an exercise to the adventurous readers.

At this point, we need to encode _state transitions_ in some way. Given a state and an event, we want to compute the next one:

```haskell
transition :: State -> Event -> State
```

But remember that we have the `Create` event, which is a _transition without a starting state_. So, the initial state must be optional:

```haskell
transition :: Maybe State -> Event -> State
```

At this point we must also consider that _not all transitions are valid from a given state_. For example, if we are in the `Approved` state, how we react to the `Reject` event? One possible option would be to NOT transition the state, but this way we would treat in the same way two very different cases:

* A valid transition in which the starting state and the destination one are the same.
* An *invalid* transition.

To differentiate those two cases and obtain a _validation_ of an ordered sequence of events in relation to our process, we make the transitioned state optional too. This way if the required transition is invalid, we'd get a `Nothing`.

```haskell
transition :: Maybe State -> Event -> Maybe State
```

Futhermore, now that the input and output states are of the same type we can easily _chain multiple transitions sequentially_ again.

The implementation is fairly straightforward:

```haskell
transition :: Maybe State -> Event -> Maybe State
transition Nothing           Create  = Just InProgress
transition (Just InProgress) Approve = Just Approved
transition (Just InProgress) Reject  = Just Rejected
transition (Just _)          Delete  = Just Deleted
transition s                 _       = Nothing
```

An advantage of encoding state transitions as a function using Haskell is that we get _completeness checking for free_: the compiler will warn you if you forget to cover some state-event combinations. Another advantage of Haskell is that we can use _pattern matching_ and concisely express some rules like:

* "Regardless of the starting state, if we get the `Delete` event the new state is `Deleted`."
* "If the state-event combination is not a valid one, there is no new state."

Now we have everything we need to implement our _projection_:

```haskell
project :: [Event] -> Maybe State
project = foldl transition Nothing
```

Here `project` takes an ordered list of events and computes the final state by sequentially applying `transition` to them. You can notice that the starting state is `Nothing` since we expect a `Create` event to transition to the `InProgress` state.

> Thanks to [Currying](https://en.wikipedia.org/wiki/Currying), I didn't need to explicitly include the `[Event]` parameter in the function implementation.

A nice property of this implementation is that we get _events stream validation for free_. If `project` returns `Nothing`, it means that the input events are incompatible with out FSM.

### Snapshots
We can also easily support snapshots with minimal changes! We just need to be able to apply a set of events to an arbitrary initial state, not... just `Nothing` (pun not intended).

We just need to generalize `project` by adding the initial state as an additional argument:

```haskell
project :: Maybe State -> [Event] -> Maybe State
project start = foldl transition start
```

And for convenience we can write another function to project all the events:

```haskell
projectAll :: [Event] -> Maybe State
projectAll = project Nothing
```

That's it! We can easily test the code using GHCi:

```
*FSM> projectAll [Create, Approve]
Just Approved
*FSM> projectAll [Create, Reject, Delete]
Just Deleted
*FSM> projectAll [Delete]
Nothing
*FSM> projectAll [Create, Approve, Reject]
Nothing
*FSM> project (Just Approved) [Delete]                         
Just Deleted
```

## Appendix: the complete solution
```haskell
module FSM where

data State
    = InProgress
    | Approved
    | Rejected
    | Deleted
    deriving (Eq,Show)

data Event
    = Create
    | Approve
    | Reject
    | Delete
    deriving (Eq,Show)

transition :: Maybe State -> Event -> Maybe State
transition Nothing Create = Just InProgress
transition (Just InProgress) Approve = Just Approved
transition (Just InProgress) Reject = Just Rejected
transition (Just _) Delete = Just Deleted
transition s _ = Nothing

project :: Maybe State -> [Event] -> Maybe State
project start = foldl transition start

projectAll :: [Event] -> Maybe State
projectAll = project Nothing
```

## Update: 2017-11-05
[Bose](https://www.bose.com/en_us/index.html) just [released](https://github.com/BoseCorp) [Smudge](https://github.com/BoseCorp/Smudge), "A domain-specific language for state machines":

> Smudge is a state machine and UML diagram generation engine.
>
> Smudge is both a language for describing state machines as well as a compiler that interprets and validates those descriptions to generate code and documentation for them.

You can find the official tutorial [here](https://github.com/BoseCorp/Smudge/blob/master/docs/tutorial/tutorial.rst).

