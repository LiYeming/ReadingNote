# Mathematizing C++ Concurrency

Key issue: multiprocessor relaxed-memory behaviour induced by hardware and compiler optimizations

- Tradeoff between efficiency and reasoning
- Rigorous mathematical semantics for C++ concurrency

## C++ concurrency model

semantics of a program $p$ will be a set of allowed executions $X$

### Single threaded programs

an execution consists of a set of memory actions and various relations over them. The memory model axiomatises constraints on those

![image-20200702191514581](/home/liyeming/.config/Typora/typora-user-images/image-20200702191514581.png)

W for write, R for read, na for non-atomic, sb for sequenced-before, rf for read from

In a ``non-SC semantics``, the constraints on reads cannot be simply that they read from the `most recent' write. They are constrained by ``happens-before`` relationship

Non-atomic reads have to read from a ```visible side effect```, a write to the same location that happens-before the read but is not happens-before-hidden

### Threads, Data Races, and Locks

![image-20200702193244197](/home/liyeming/.config/Typora/typora-user-images/image-20200702193244197.png)

Data races can be prevented by using `mutexes`

- `lock` and `unlock` memory actions on `mutex` location
- For multi-threaded programs with locks but without atomics
  - happens-before is the transitive closure of the union of the sequenced-before and synchronizes-with relations
  - The definition of a `visible side effect` and the conditions on the `reads-from` relation are unchanged from the single-threaded case

### SC Atomics

For simple concurrent accesses to shared memory that are not protected by locks, C++ provides `sequentially consistent atomics`.

SC atomic operations are totally ordered by sc, and so can be thought of as interleaving with each other in a global time-line.

### Low-level Atomics

SC atomics are expensive to implement on most multiprocessors. Provides more synchronization than needed for many concurrent idioms.

`memory order` introduces weaker variants, specifies how much synchronization and ordering is required.

- relaxed
- release
- acquire
- consume
- acq_rel

### Types and Relations

![image-20200702195004228](/home/liyeming/.config/Typora/typora-user-images/image-20200702195004228.png)

### Release / Acquire Synchronization

`release` actions

- atomic write or fence with memory order release, acq_rel or seq_cst

`acquire` actions

- atomic read or fence with memory order acquire, acq_rel or seq_cst



Programming idiom:

![image-20200702195543929](/home/liyeming/.config/Typora/typora-user-images/image-20200702195543929.png)

The desired guarantee here is that the receiver must see the data writes of the sender.

For such programs, happens-before is still the transitive closure of the union of the sequenced-before and synchronizes-with relations.

A write-release synchronizes-with a read-acquire if both act on
the same location and the release sequence of the release contains release-sequence the write that the acquire reads from.

![image-20200702210544102](/home/liyeming/.config/Typora/typora-user-images/image-20200702210544102.png)

### Constraining Atomic Read Values

The values that can be read by an atomic action depend on happens-before, derived from sequenced-before and synchronizes-with.



An atomic action must read a write that is in one of its `visible sequences of side effects`.

We define `visible-sequences-of-side-effects` to be the binary relation relating atomic reads to their `visible-side-effect sets` (now including the visible side effects themselves). The atomic read must read from a write in one of these sets.



A candidate execution is required to be free from the following four execution fragments. This property is called `coherence`.

- CoRR, two reads ordered by happens-before may not read two writes that are modification ordered in the other direction.
- CoWR, It is forbidden to read from a write that is happends-before-hidden by a later write in modification order.
- CoWW, Happens-before and modification-order may not disagree.
- CoRW, The union of the reads-from map, happens-before and modificiation-order must be acyclic.

![image-20200702212059748](/home/liyeming/.config/Typora/typora-user-images/image-20200702212059748.png)

### Putting it together

> Theorem, If a consistent execution has no consume operations, happens-before is transitive

## Examples

**Store Buffering (SB)**

two threads write to separate locations and then each reads from the other location.

- allowed for relaxed
- not allowed for other.

**Message Passing (MP)**

![image-20200702215055515](/home/liyeming/.config/Typora/typora-user-images/image-20200702215055515.png)

with a release/acquire pair, it is guaranteed for e to see b.

**Load Buffering (LB)**

![image-20200702214753141](/home/liyeming/.config/Typora/typora-user-images/image-20200702214753141.png)

With relaxed atomics, this is allowed. It is not allowed for release/acquire and sequentially consistent atomics.

**Write-to-Read Causality (WRC)**

![image-20200702214356867](/home/liyeming/.config/Typora/typora-user-images/image-20200702214356867.png)

With relaxed atomics, g is not guaranteed to read c.

**Independent Reads of Independent Writes (IRIW)** 

![image-20200702214047735](/home/liyeming/.config/Typora/typora-user-images/image-20200702214047735.png)

with relaxed, release/acquire, release/consume atomics, they can see different orders.



