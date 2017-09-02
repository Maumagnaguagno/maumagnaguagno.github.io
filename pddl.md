---
title: PDDL
description: The Planning Domain Definition Language
date: 2016-09-01
category: pddl
---

# PDDL

## Why PDDL exists?
- Many different languages/inputs formats for each planner
  - Incompatibility made planners hard to compare and utilize
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
  - ``ball``, ``room1``, ``hero``, ``sword``
- **Predicates**: Properties of objects that we are interested in (can be true or false)
  - ``(drunk hero)``
- **State**: Set of properties at some point
  - ``( (drunk hero) (at hero room1) )``
- **Initial state**: The state of the world that we start in
- **Goal specification**: The parts of a state we want to be true (ignore the rest)
- **Actions/Operators**: Ways of changing the state of the world
  - **Parameters and Variables**: Used to reference objects
- **Domain**: Set of actions that define a scenario
- **Problem**: Objects, initial and goal states that define an instance of the scenario

## Domain
```elisp
; Domain - this is a comment
(define (domain hanoi)
  (:requirements :strips :negative-preconditions :equality)
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

```elisp
(:action name
  :parameters (
    ?param1 ?param2 - type ; requires typing
    ?param3 ?param4 ; strips
  )
  :precondition (and
    (pred ?param1) ; strips
    (not (pred ?param2)) ; negative-preconditions
    (not (= ?param1 ?param2)) ; negative-preconditions and equality
    (or ; disjunctive-preconditions
      (pred ?param3)
      (pred ?param4)
    )
  )
  :effect (and
    (not (pred ?param1)) ; strips
    (pred ?param2) ; strips
    (when ; conditional-effects
      (pred2 ?param1)
      (not (pred2 ?param1))
    )
  )
)
```

## Downgrading PDDL
- Some planners may lack the desired requirements
- Sometimes we can rewrite the description using simpler constructions
- ``:negative-preconditions``
  - Duplicate predicate and use antonym instead, sometimes you can remove the original predicate
  - ``(not (clean ?space))`` to ``(dirty ?space)``
- ``:equality``
  - Add an equality predicate ``(equal obj obj)`` for each object at the initial state and replace preconditions
- ``:typing``
  - Move types to initial state and parameters to preconditions
  - ``(?var - type)`` to ``(type ?var)``
- ``:disjunctive-preconditions``
  - Break each part of the precondition in a different action

## What is important to report/document?
- Which requirements you used?
  - Why?
  - Do they make it simpler to describe/understand the problem?
- Which predicates you used?
- Small modifications impact your description?
  - Typing
  - More objects
  - More predicates in goal

## PDDL and LaTeX listings
You can add PDDL to your papers/reports using LaTeX listings with a custom template.
To simplify this process I created a [repository](https://github.com/Maumagnaguagno/Planning_LaTeX "Planning LaTeX") to contain listings for planning and usage example.

## References
- [PDDL 2.1 specification](http://www.public.asu.edu/~ktalamad/tmp/files/pddl21specs.pdf)
- [An Introduction to PDDL, by Malte Helmert](http://www.cs.toronto.edu/~sheila/2542/s14/A1/introtopddl2.pdf)