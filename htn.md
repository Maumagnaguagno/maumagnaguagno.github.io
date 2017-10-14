---
title: HTN
description: Hierarchical Task Network.
date: 2017-09-07
category: planning
hidden: true
---

# HTN

**TODO add intro here**

## Elements
**TODO list elements**
- Primitives/operators
- Non-primitives/methods

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