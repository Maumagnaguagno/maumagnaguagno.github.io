---
title: PDDL
description: The Planning Domain Definition Language
date: 2016-09-01
category: planning
---

# PDDL
Many different languages/inputs formats for each planner created an environment where incompatibility made planners hard to be compared and rework was required to use other planners.
Planning community had no reason to use same language, there was no well defined formalism (only [STRIPS](https://en.wikipedia.org/wiki/STRIPS)-like) and they were interested in fast/correct planners, not being limited by parsers.
It is time to make planners compete fot the title of best planner and while publishing their approaches, [ICAPS](http://www.icaps-conference.org/).
- Make planners [compete](http://icaps-conference.org/index.php/main/competitions) within the same rules/input ([PDDL 1.2 from 98](http://homepages.inf.ed.ac.uk/mfourman/tools/propplan/pddl.pdf))
- May the best/faster/more compatible planner win
- Force community to share language/limitations/benchmarks

Some people were not happy with this as their planners required more than PDDL could express, their incompatible planners were forgotten due to lack of usage and documentation.
PDDL is incremented from time to time to support new features, which may be modified or dropped as implementations and use cases appear.

## Planning elements
- **Objects**: elements in the world that are of our interest
  - ``ball``, ``room1``, ``hero``, ``sword``
- **Predicates**: Properties of objects that can be true or false
  - ``(drunk hero)``
- **State**: Set of true predicates
  - ``( (drunk hero) (at hero room1) )``
- **Initial state**: State of the world that we start in
  - Usually with [closed-world assumption](https://en.wikipedia.org/wiki/Closed-world_assumption), predicates not mentioned are false
- **Goal specification**: Partial state we want to be achieve
  - Predicates not mentioned are ignored
- **Actions/Operators**: Rules that change a state
  - **Parameters**: Used to generalize, will be replaced by objects
  - **Precondition**: When a rule can applied (satisfied by current state)
  - **Effect**: What changes (add or delete predicates from current state)
- **Domain**: Set of actions that define a scenario
- **Problem**: Objects, initial and goal states that define an instance of the scenario

## Domain
An implementation of the [Tower of Hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi) is a good start, there is only one action and very clear initial and goal states.
It starts with several disks in the leftmost of three pegs, each disk on top of a larger one.
The goal is to move all disks to the rightmost peg, but there is a catch, only one disk can be moved at a time and never onto a smaller one.
Three predicates are used to describe if the top of a disk is ``clear``, which disk is ``on`` top of another and which disk is ``smaller`` than another.
The ``move`` action has three parameters/free variables, prefixed by ``?``.

```elisp
(define (domain hanoi) ; This is a comment, there are no multiline comments
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
For a problem with only three disks there are several ``smaller`` predicates in the initial state to describe which movements are possible.
The disks are on top of each other on ``peg1`` with the smaller ``disk1``, ``peg2`` and ``peg3`` being ``clear``.
The goal only have the stack on ``peg3``, but one could add ``(clear peg1)`` and ``(clear peg2)``.
For more or less disks only a different problem description is needed, as they are new problem instances of the same domain.

```elisp
(define (problem pb3) ; This is also a comment
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
    (on d1 d2) (on d2 d3) (on d3 peg3)
  ))
)
```

## Requirements
Planners usually do not support every PDDL specification out there, as each version adds expressive power to support domains that otherwise would not be able to be described in PDDL.
The expressive power is handled in layers of **requirements** in PDDL.
Not to mention the many experimental requirements that are published without sufficient implementation details.
A planner that support everything is too complex for developers that are interested in a subset of PDDL that solves their problem, they need to focus on something they can maintain and optimize for their use case.
It is left to the user to select ideal/correct planner based on their requirements:
- What is needed to better describe domain and problem?
  - Use the ``:requirements`` field correctly
- What is expected?
  - Satisficing plan (sub-optimal, fast)
  - Step-optimal plan (optimal, slow)
  - Metric-based plan (minimize/maximize time/cost/resource, slower)

Common requirements:
- ``:strips`` - default requirement (positive preconditions, add and delete effects)
- ``:negative-preconditions`` - able to use ``not`` in preconditions
- ``:equality`` - able to use ``=`` in preconditions
- ``:typing`` - able to type objects and variables, ``obj - type``
- ``:disjunctive-preconditions`` - able to use ``or`` in preconditions
- ``:conditional-effects`` - able to use ``when`` in effects

Some planners may lack the desired requirements.
instead of giving up and searching for a new planner, we can rewrite the PDDL description using simpler constructions.
These are a few tricks to **downgrade** PDDL:
- ``:negative-preconditions``
  - Duplicate predicate and use antonym instead, remove the original predicate if possible
  - ``(not (clean ?space))`` to ``(dirty ?space)``
- ``:equality``
  - Add ``(equal obj obj)`` for each object at the initial state and replace preconditions
  - ``(= ?a ?b)`` to ``(equal ?a ?b)``
- ``:typing``
  - Move types and subtypes to initial state and parameters to preconditions
  - ``(?var - type)`` to ``(type ?var)``
- ``:disjunctive-preconditions``
  - Break each side of the disjunction in a different action
  - ``(and (p0 a) (or (p1 b) (p2 c)))`` to ``(and (p0 a) (p1 b))``, ``(and (p0 a) (p2 c))``

The following action shows how most common requirements take place:

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
    (when (pred2 ?param1) ; conditional-effects
      (not (pred2 ?param1))
    )
  )
)
```

## Documentation
Once domain and problems is done it is important to document the design decisions that were taken.
But what is important to report/document?
- Which planner is compatible with the description?
  - Describe version and flags used to execute
- Which requirements and predicates were used?
  - Why?
  - Do they make it simpler to describe/understand the problem?
- Small modifications impact the description/planning?
  - Typing
  - More objects
  - More predicates in goal

## LaTeX listings
It is possible to add syntax highlighted PDDL to your papers/reports using LaTeX listings with a custom template.
To simplify this process I created a [repository](https://github.com/Maumagnaguagno/Planning_LaTeX "Planning LaTeX") with listings for planning descriptions.

## References
- [PDDL 2.1](https://helios.hud.ac.uk/scommv/IPC-14/repository/fox-long-jair-2003.pdf) by Fox and Long
- [An Introduction to PDDL](https://www.cs.toronto.edu/~sheila/2542/s14/A1/introtopddl2.pdf) by Malte Helmert
- [AI Planning with STRIPS](http://www.primaryobjects.com/2015/11/06/artificial-intelligence-planning-with-strips-a-gentle-introduction/) by Kory Becker