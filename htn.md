---
title: HTN
description: Hierarchical Task Network.
date: 2017-09-07
category: planning
---

# HTN
Hierarchical Task Network is one approach to automated planning.
Differently from classical planning, where one must find a plan to go from a initial state to a goal state, HTNs work decomposing tasks based on the current state.
This is a shift from explicit to implicit goal state, instead of ``(at hero there)`` we now have ``(travel hero there)``.
In this case a task ``travel`` must be performed, which is decomposed to a sequence of ``move`` tasks, that are no longer decomposeable.
Tasks like ``travel`` are non-primitives or methods, and may have several ways to be decomposed, while tasks like ``move`` are primitives or operators and make the plan that modify the state.

## Implementations
- [SHOP and JSHOP](https://www.cs.umd.edu/projects/shop/)
- [PyHop](https://bitbucket.org/dananau/pyhop)
- [HyperTensioN](https://github.com/Maumagnaguagno/HyperTensioN)
- [HyperTensioN_U](https://github.com/Maumagnaguagno/HyperTensioN_U)

## Search example

### Problem
Since both PDDL and JSHOP input based on Lisp and developed around the same time they share style (lots of parentheses), but not keywords.
PDDL is more verbose with each field being named, while JSHOP expects the user to remember which field is what.
The problem file is almost identical, the main differences are the lack of explicit objects and a task list instead of a goal state.
The missing keywords can be obtained with a smart use of comments.

<div class="split" markdown="1">

#### PDDL
```elisp
(def (problem pb1) (:domain search)
  (:objects ag1 p0 p1 p2 p3 p4)
  (:init
    (at ag1 p0)
    (adjacent p0 p1) (adjacent p1 p0)
    (adjacent p1 p2) (adjacent p2 p1)
    (adjacent p2 p3) (adjacent p3 p2)
    (adjacent p3 p4) (adjacent p4 p3)
  )
  (:goal
    (at ag1 p4)
  )
)
```

</div>
<div class="split" markdown="1">

#### JSHOP
```elisp
(defproblem pb1 search

  (; initial state
    (at ag1 p0)
    (adjacent p0 p1) (adjacent p1 p0)
    (adjacent p1 p2) (adjacent p2 p1)
    (adjacent p2 p3) (adjacent p3 p2)
    (adjacent p3 p4) (adjacent p4 p3)
  )
  (; task list
    (forward ag1 p4)
  )
)
```

</div>

### Move operator
JSHOP operators are prefixed with ``!``.
Instead of preconditions and effects, JSHOP operators have preconditions, delete and add effects.
The conjunction token ``and`` can be omitted.

<div class="split" markdown="1">

#### PDDL
```elisp
(:action move
  :parameters (?agent ?from ?to)
  :precondition (and
    (at ?agent ?from)
    (adjacent ?from ?to)
  )
  :effect (and
    (not (at ?agent ?from))
    (at ?agent ?to)
  )
)

```

</div>
<div class="split" markdown="1">

#### JSHOP
```elisp
(:operator (!move ?agent ?from ?to)
  (; preconditions
    (at ?agent ?from)
    (adjacent ?from ?to)
  )
  (; delete effects
    (at ?agent ?from)
  )
  (; add effects
    (at ?agent ?to)
  )
)
```

</div>

### Mark to avoid repetition
To avoid reaching the same position multiple times a memory is needed.
Memory for planners is defined as a list of entire or partially states visited during search.
Search algorithms and some classical planners employ the entire state option, being implicit that any reached state will not be explored twice.
HTNs on the other hand may have tasks that must reach the same state multiple times and require the planning instance description to explicitly avoid repetions when undesired.
To create this memory of what already happened we can add predicates that mark partial states, in this case positions and agents, that were already explored.
Since only operators can modify the state we need to add them, but they are only important internally, we can make them invisible to the final plan prefixing with ``!!``.

<div class="split" markdown="1">

```elisp
(:operator (!!visit ?agent ?pos)
  () ; preconditions
  () ; delete effects
  ((visited ?agent ?pos)) ; add effects
)
```

</div>
<div class="split" markdown="1">

```elisp
(:operator (!!unvisit ?agent ?pos)
  () ; preconditions
  ((visited ?agent ?pos)) ; delete effects
  () ; add effects
)
```

</div>

### Forward search
```elisp
(:method (forward ?agent ?goal)
  base
  ((at ?agent ?goal)) ; preconditions
  () ; subtasks

  recursion
  (; preconditions
    (at ?agent ?from)
    (adjacent ?from ?place)
  )
  (; subtasks
    (!move ?agent ?from ?place)
    (!!visit ?agent ?from)
    (forward ?agent ?goal)
    (!!unvisit ?agent ?from)
  )
)
```