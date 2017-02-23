---
title: PDDL
description: The Planning Domain Definition Language
date: 2016-09-01
category: pddl
---

# PDDL

## Why PDDL exists?
- Many different languages/inputs
  - Incompatibility made planners hard to compare
- Planning community had no reason to use same language
  - There was no one formalism well defined, only STRIPS
  - They were interested in fast/correct planners, not being limited by parsers

- [ICAPS](http://www.icaps-conference.org/)
  - Make planners [compete](http://icaps-conference.org/index.php/main/competitions) within the same rules/input (PDDL 1 from 98)
  - May the best/fast/compatible planner win
  - Force community to share language/limitations/benchmarks
    - Some people were not happy with this (incompatible planners were forgotten)

## Planning elements
- **Objects**: Things in the world that interest us
  - ``ball, room1, hero, sword``
- **Predicates**: Properties of objects that we are interested in (can be true or false)
  - ``(drunk hero)``
- **State**: Set of properties at some point
  - ``( (drunk hero) (at hero room1) )``
- **Initial state**: The state of the world that we start in
- **Goal specification**: Things that we want to be true
- **Actions/Operators**: Ways of changing the state of the world
  - **Parameters and Variables**: Used to reference an object, unification is left to the planner

## Domain
```elisp
; Domain - this is a comment
(define (domain hanoi)
  (:requirements :strips :negative-preconditions :equality )
  (:predicates (clear ?x) (on ?x ?y) (smaller ?x ?y) )
  (:action move
    :parameters (?disc ?from ?to)
    :precondition (and
       (smaller ?disc ?to) (smaller ?disc ?from)
       (on ?disc ?from)
       (clear ?disc) (clear ?to)
       (not (= ?from ?to))
    )
    :effect (and
      (clear ?from)
      (on ?disc ?to)
      (not (on ?disc ?from))
      (not (clear ?to))
    )
  )
)
```

## Problem
```elisp
; Problem - this is also a comment
(define (problem pb3)
  (:domain hanoi)
  (:objects peg1 peg2 peg3 d1 d2 d3)
  (:init
    (smaller d1 peg1) (smaller d1 peg2) (smaller d1 peg3)
    (smaller d2 peg1) (smaller d2 peg2) (smaller d2 peg3)
    (smaller d3 peg1) (smaller d3 peg2) (smaller d3 peg3)
    (smaller d1 d2) (smaller d1 d3)
    (smaller d2 d3)
    (clear d1) (clear peg2) (clear peg3)
    (on d1 d2) (on d2 d3) (on d3 peg1)
  )

  (:goal (and
    (on d1 d2)
    (on d2 d3)
    (on d3 peg3)
  ))
)
```

## Matching PDDL and planner
- Planners usually do not support full PDDL specifications
  - Too much, hard to maintain and optimize
- User must select ideal/correct planner based on requirements
  - What do you need to describe domain and problem?
    - The ``:requirements`` field is your friend
  - What do you expect as output?
    - Any plan (sub-optimal)
    - Step-optimal plan
    - Metric-based plan (minimize/maximize time/cost/resource)

## Most common requirements
- ``:strips`` - default requirement (positive preconditions, add and delete effects)
- ``:negative-preconditions`` - able to use ``not`` in preconditions
- ``:equality`` - able to use ``=`` in preconditions
- ``:typing`` - able to type objects and variables, ``obj - type``
- ``:disjunctive-preconditions`` - able to use ``or`` in preconditions
- ``:conditional-effects`` - able to use ``when`` in effects

## Downgrading PDDL
- Some planners may lack the desired requirements
- Sometimes we can rewrite the description using simpler constructions
- ``:negative-preconditions``
  - Duplicate predicate and use antonym instead, sometimes you can remove the original predicate
  - ``(not (clean ?space))`` => ``(dirty ?space)``
- ``:equality``
  - Add an equality predicate ``(equal obj obj)`` for each object at the initial state and replace preconditions
- ``:typing``
  - Move types to initial state and parameters to preconditions
  - ``(?var - type)`` => ``(type ?var)``
- ``:disjunctive-preconditions``
  - Break each part of the precondition in a different action

## What is important to describe?
- Which requirements you used?
  - Why?
  - Do they make it simpler to describe/understand the problem?
- Which predicates you used?
- Small modifications impact your description?
  - Typing
  - More objects
  - More predicates in goal


## LaTeX
In order to add PDDL and SHOP snippets to your tex files you can extend Lisp with the following setup.
```tex
\usepackage{listings}

\lstdefinelanguage
{PDDL}[]{LISP}{
alsoletter={:,?},
morekeywords={
  define,domain,problem,:domain,:extends,:requirements,:strips,
  :equality,:typing,:conditional-effects,:types,:objects,
  :negative-preconditions,:disjunctive-preconditions,
  :existential-preconditions,:universal-preconditions,
  :constants,:domain-variables,:timeless,:domain-axioms,
  :action-expansions,:predicates,:action,:parameters,:precondition,
  :effect,:fluents,:primary-effect,:side-effect,:init,:goal,when,not,
  forall,exists,goal-type,choice,forsome,in-context
}}

\lstdefinelanguage
{SHOP}[]{LISP}{
alsoletter={:,?,!},
morekeywords={
  defdomain,defproblem,:domain,:method,:operator,:goal,
  when,not,forall,exists,goal-type,choice,forsome,in-context
}}
```

And use like this:
```tex
\begin{lstlisting}[
  float=!tb,
  caption={Move operator.},
  label={lst:pddlmove},
  language=PDDL]
(:action move
  :parameters (?bot - robot ?source ?destination - hallway)
  :precondition (and
    (at ?bot ?source)
    (not (at ?bot ?destination))
    (connected ?source ?destination)
  )
  :effect (and
    (not (at ?bot ?source))
    (at ?bot ?destination)
  )
)
\end{lstlisting}
```

## References
- [PDDL 2.1 specification](http://www.public.asu.edu/~ktalamad/tmp/files/pddl21specs.pdf)
- [An Introduction to PDDL, by Malte Helmert](http://www.cs.toronto.edu/~sheila/2542/s14/A1/introtopddl2.pdf)