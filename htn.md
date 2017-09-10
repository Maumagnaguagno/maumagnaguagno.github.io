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
- SHOP and JSHOP
- PyHop
- HyperTensioN
- HyperTensioN_U
**TODO**

## Search examples

### The move operator

<div class="split"><div markdown="1">

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

</div><div markdown="1">

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

</div></div>

### Mark states to avoid repetition
<div class="split"><div markdown="1">

```elisp
(:operator (!!visit ?agent ?pos)
  () ; preconditions
  () ; delete effects
  ((visited ?agent ?pos)) ; add effects
)
```

</div><div markdown="1">

```elisp
(:operator (!!unvisit ?agent ?pos)
  () ; preconditions
  ((visited ?agent ?pos)) ; delete effects
  () ; add effects
)

```

</div></div>

## References
**TODO**