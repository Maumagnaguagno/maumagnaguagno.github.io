---
title: HTN
description: Hierarchical Task Network.
date: 2017-09-07
category: planning
hidden: true
---

# HTN
Hierarchical Task Network is one approach to automated planning.
Differently from classical planning, where one must find a plan to go from the initial state to a goal state, HTNs work decomposing tasks.
This is a shift from explicit to implicit goal state, instead of ``(at hero there)`` we now have ``(travel hero there)``.
In this case a task ``travel`` must be performed, which is decomposed to a sequence of ``move`` tasks, that are no longer decomposeable.
Tasks like ``travel`` are non-primitives or methods, and may have several ways to be decomposed, while tasks like ``move`` are primitives or operators and make the plan that modify the state.

## Implementations
- [SHOP and JSHOP](https://www.cs.umd.edu/projects/shop/)
- [PyHop](https://bitbucket.org/dananau/pyhop)
- [HyperTensioN](https://github.com/Maumagnaguagno/HyperTensioN)
- [HyperTensioN_U](https://github.com/Maumagnaguagno/HyperTensioN_U)

## Search examples

### The move operator

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

### Mark states to avoid repetition
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
    (forward ?agent ?goal)
  )
)
```

## References
**TODO**