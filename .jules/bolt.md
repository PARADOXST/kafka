## 2024-05-24 - Map.keySet() followed by .get()
**Learning:** Found several instances in hot paths like `StickyTaskAssignor.java` and `PartitionStates.java` where Maps were iterated using `for (Key k : map.keySet()) { Value v = map.get(k); ... }`. This anti-pattern does a redundant hash lookup on every iteration, which is costly in large Kafka topologies.
**Action:** Always scan for `keySet()` loops that call `.get()` inside, and refactor them to use `.entrySet()` for single-pass iteration.
