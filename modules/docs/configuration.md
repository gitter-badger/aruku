---
id: configuration
title: "Configuration"
---

## Walker 

A walker is a simple scala case class that contains an id, a step count and a data :

```scala title="modules/aruku/Walkerscala"
case class Walker[T](
  id: Long,
  step: Long,
  data: T
)
```

But you can't make one by yourself, they are automatically generated by aruku thanks to the walker configuration.

## Walker Configuration

The walker configuration defines how many walkers will be doing random walks, how a walker is initialized, how it's updated and how it's affected to a vertice.

```scala title="modules/aruku/WalkerConfig.scala"
case class WalkerConfig[T](
  numWalkers: Long,
  init: VertexId => T,
  update: (Walker[T], VertexId, Edge[Double]) => T,
  start: StartingStrategy
)
```
 
It can be either constant, as the walker is not updated at every step. DeepWalk is one example of such a walker configuration :

```scala title="modules/aruku/WalkerConfig.scala"
object WalkerConfig {

  def constant[T](
    numWalkers: Long,
    init: VertexId => T,
    start: StartingStrategy
  )

}
```

Or dynamic if we need to update the data of the walker at every step.

```scala title="modules/aruku/WalkerConfig.scala"
object WalkerConfig {

  def dynamic[T](
    numWalkers: Long,
    init: VertexId => T,
    update: (Walker[T], VertexId, Edge[Double]) => T,
    start: StartingStrategy
  ) 

}
```


## Walker Engine Configuration

You can configure the engine itself at the moment with the ```"spark.graphx.pregel.checkpointInterval"``` at the moment in order to break the lineage of the random walk if you are doing a lot of steps.