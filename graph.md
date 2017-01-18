# Connectivity Graph

### Is a node connected?

- has provisional -> yes
- has manual -> inherit 
- ranked-only -> no
- is author -> yes

### Node gains connectivity - do children flip?

- ranked, with other provisional -> no
- ranked, with other manual -> no
- manual -> yes
- ranked, with ranked only -> yes
- (guaranteed zero incoming provisional)

### Node loses connectivity - do children flip?

- ranked -> no
- provisional -> maybe
- manual -> yes

## Cyclical Connectivity

- influence -> 0
- connected -> 0

## Flip connectivity (connect) algorithm.

Connectivity can be gained when:
- a ranked-only node connects directly to a non-adjacent opinion 
- a ranked-only node composes an opinion
- a ranked-only node ranks (for the first time) a connected node
- a manual connection to an unconnected node is removed and a provisional connection added
- a manual connection to an unconnected node is swapped for a manual connection to a connected node

Any connection switch requires a cycle check

``` 
flipGainedConnection(incomingRelationship) -> {
  childNode = incomingRelationship.getStart()

  if (incomingRelationship.isManual) -> 
    childNode.getIncoming().map(flipGainedConnection)
  
  if (incomingRelationship.isRanked) ->
    if (childNode.isManual) -> return

    if (childNode.isProvisionedBetter) -> return

    if (childNode.isProvisionedWorse) -> 
      set provisioned = incomingRelationship
      
      if (!isCycle(childNode)) -> return

      if (isCycle(childNode)) -> 
        unProvisionCycle() 
        gatherCycleIncoming().map(flipLostConnection)
        return

    // _new_ best ranked (so flip children)
    set provision = incomingRelationship
    child.getIncoming().map(flipGainedConnection)
    return
}
```

## Flip Connectivity (disconnect) algorithm

Connectivity can be lost when:
- a provisional connection is swapped for a manual connection to an unconnected node
- a manual connection to a connected node is swapped for a manual connection to an unconnected node
- a manual connection to a non-adjacent node is removed (no remaining ranked nodes are connected)
- a node with a provisional connection un-ranks the provisionally connected node (no remaining ranked nodes are connected)
- ranked-only == not possible

```
flipLostConnection(incomingRelationship) ->
  childNode = incomingRelationship.getStart()

  // didn't use this, so won't flip
  if (incomingRelationship.isRanked) -> return

  // used this, so need to check children
  if (incomingRelationship.isManual) ->
    childNode.getIncoming().map(flipLostConnection)

  // used this, so guaranteed to change; may flip
  if (incomingRelationship.isProvisional)
    remove provision

    // try to provision a new connection
    newProvision = provisionConnection(childNode)

    // flipped, so check children
    if (!newProvision)
      childNode.getIncoming().map(flipLostConnection)
      return

    if (newProvision)
      set provision = newProvision
 
      // cycle means no connection means flipped!
      if (isCycle(childNode))
        unProvisionCycle() 
        gatherCycleIncoming().map(flipLostConnection)
        return

      // re-provisioned, but didn't create cycle (and flip)
      if (!isCycle(childNode))
        return
```

## Cycle detection (after pointer change)

```
modified node ->

current = node;

while(current.hasNext())
  current = current.getNext()
  if (current == node) return true

return false
```

## Re-ranking

Same as last part of flipLostConnection

```
rerank(node) ->
  unProvision(node)
  
  newProvision = provisionConnection(node);

  if (!newProvision)
    node.getIncoming().map(flipLostConnection)

  if (newProvision)
    set provision = newProvision

    // cycle means no connection means flipped!
    if (isCycle(node))
      unProvisionCycle() 
      gatherCycleIncoming().map(flipLostConnection)
      return

    // re-provisioned, but didn't create cycle (and flip)
    if (!isCycle(node))
      return
```

## Manual Selection

```
choose(node, proxy) ->
  wasConnected = node.isConnected()

  connect(node, proxy)

  // cycle means no connection
  // doesn't matter if flipped?
  if (isCycle(node))
    unProvisionCycle() 
    gatherCycleIncoming().map(flipLostConnection)
    return

  isConnected = node.isConnected()

  if (wasConnected && !isConnected) 
    node.getIncoming().map(flipLostConnection)

  if (!wasConnected && isConnected)
    node.getIncoming().map(flipGainedConnection)
```

TODO:
x make vocabulary consistent
x visualize root node of the tree at the bottom (where influence gathers)
x incoming connections come from 'upstream'
x outgoing connections flow 'downstream'


