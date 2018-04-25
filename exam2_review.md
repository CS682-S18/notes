Exam 2 Review
=============

Exam 2 will cover all material introduced so far in the class. This includes any readings or resources posted on the website (slides, notes, web links, code samples) as well as any topics we have covered in class. It will primarily focus on topics covered since Exam 1, however you may be asked about material from the first half of the class. 

The list of topics may include but is not limited to the following:

#### Replication

- Passive vs active
- Synchronous vs asynchronous
- Consistency models (read-your-writes, monotonic reads)
- Preserving causality (logical timestamps)


#### Facebook Paper

- System architecture
- Consistency models (linearizability, per-object sequential, read-after-write)
- Trace collection approach
- Linearizability checker (stale read and total order anomalies)
- Analysis and results

#### Dynamo

- System architecture
- Replication
- Versioning
- read/write
- Membership
- Analysis and results

#### Coordination and Agreement

- Definitions
  * correct process
  * crash failure
  * byzantine failure
  * safety 
  * liveness
  * causal ordering
- Mutual exclusion (central server, token ring, mutlicast/logical clocks)
- Election (bully)

#### Transactions
 
- ACID
- Anomalies (lost updates, inconsistent retrievals, read skew, dirty reads, dirty writes, premature writes, write skew)
- Serial equivalence
- Locking (granularity, when to release locks)
- Snapshots
- Snapshot isolation
- Two-phase locking
- Serializable snapshot isolation
- Deadlock (preventing, detecting)
- Distributed transactions and two-phase commit
 
#### Consensus/Raft
 
- General overview
- Terms
- Elections
- Log commits
- Repairing logs
- Config changes

#### Paxos

- Compare to Raft
- Failure after agreement

#### Spanner

- System architecture
- True Time API
- Timestamp assignment
- Analysis and results
- See discussion questions
