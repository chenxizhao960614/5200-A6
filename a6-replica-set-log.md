## Step 3: Start the Replica Set

### Command:
```bash
(base) chenxizhao@zhaochenxideMacBook-Pro replica-set % docker-compose up --wait
[+] Running 11/11
 ✔ mongo1 Pulled                                                                                                        19.5s 
 ✔ mongo2 Pulled                                                                                                        19.5s 
   ✔ e3366dd68755 Pull complete                                                                                          3.4s 
   ✔ a6af9b7b3e86 Pull complete                                                                                          3.4s 
   ✔ a99a34a965cb Pull complete                                                                                          3.6s 
   ✔ 8cf83cf22909 Pull complete                                                                                          3.7s 
   ✔ cf24ef7e4830 Pull complete                                                                                          3.7s 
   ✔ bbbeb3fddb7c Pull complete                                                                                          3.7s 
   ✔ 4eebf4571749 Pull complete                                                                                         15.6s 
   ✔ 44034023f7c5 Pull complete                                                                                         15.6s 
 ✔ mongo3 Pulled                                                                                                        19.5s 
[+] Running 10/10
 ✔ Network replica-set_default         Created                                                                           0.0s 
 ✔ Volume "replica-set_mongo1_data"    Created                                                                           0.0s 
 ✔ Volume "replica-set_mongo1_config"  Created                                                                           0.0s 
 ✔ Volume "replica-set_mongo2_data"    Created                                                                           0.0s 
 ✔ Volume "replica-set_mongo2_config"  Created                                                                           0.0s 
 ✔ Volume "replica-set_mongo3_data"    Created                                                                           0.0s 
 ✔ Volume "replica-set_mongo3_config"  Created                                                                           0.0s 
 ✔ Container replica-set-mongo1-1      Healthy                                                                           1.4s 
 ✔ Container replica-set-mongo3-1      Healthy                                                                           1.4s 
 ✔ Container replica-set-mongo2-1      Healthy                                                                           1.4s 
(base) chenxizhao@zhaochenxideMacBook-Pro replica-set % docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                 NAMES
ff110bbd3435   mongo:latest   "docker-entrypoint.s…"   18 seconds ago   Up 17 seconds   0.0.0.0:27017->27017/tcp              replica-set-mongo1-1
ba35c3571d55   mongo:latest   "docker-entrypoint.s…"   18 seconds ago   Up 17 seconds   27017/tcp, 0.0.0.0:27019->27019/tcp   replica-set-mongo3-1
d39af109c258   mongo:latest   "docker-entrypoint.s…"   18 seconds ago   Up 17 seconds   27017/tcp, 0.0.0.0:27018->27018/tcp   replica-set-mongo2-1
```
## Step 5: Initialize the Replica Set

### Command:
```bash
rs0 [direct: secondary] test> rs.status();
{
  set: 'rs0',
  date: ISODate('2024-11-18T00:13:53.968Z'),
  myState: 1,
  term: Long('1'),
  syncSourceHost: '',
  syncSourceId: -1,
  heartbeatIntervalMillis: Long('2000'),
  majorityVoteCount: 2,
  writeMajorityCount: 2,
  votingMembersCount: 3,
  writableVotingMembersCount: 3,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
    lastCommittedWallTime: ISODate('2024-11-18T00:13:50.103Z'),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
    appliedOpTime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
    durableOpTime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
    writtenOpTime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
    lastAppliedWallTime: ISODate('2024-11-18T00:13:50.103Z'),
    lastDurableWallTime: ISODate('2024-11-18T00:13:50.103Z'),
    lastWrittenWallTime: ISODate('2024-11-18T00:13:50.103Z')
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1731888790, i: 1 }),
  electionCandidateMetrics: {
    lastElectionReason: 'electionTimeout',
    lastElectionDate: ISODate('2024-11-17T23:52:28.583Z'),
    electionTerm: Long('1'),
    lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1731887538, i: 1 }), t: Long('-1') },
    lastSeenWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731887538, i: 1 }), t: Long('-1') },
    lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1731887538, i: 1 }), t: Long('-1') },
    numVotesNeeded: 2,
    priorityAtElection: 1,
    electionTimeoutMillis: Long('10000'),
    numCatchUpOps: Long('0'),
    newTermStartDate: ISODate('2024-11-17T23:52:28.637Z'),
    wMajorityWriteAvailabilityDate: ISODate('2024-11-17T23:52:29.126Z')
  },
  members: [
    {
      _id: 0,
      name: 'replica-set-mongo1-1:27017',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 6943,
      optime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeDate: ISODate('2024-11-18T00:13:50.000Z'),
      optimeWritten: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeWrittenDate: ISODate('2024-11-18T00:13:50.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastDurableWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastWrittenWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: '',
      electionTime: Timestamp({ t: 1731887548, i: 1 }),
      electionDate: ISODate('2024-11-17T23:52:28.000Z'),
      configVersion: 1,
      configTerm: 1,
      self: true,
      lastHeartbeatMessage: ''
    },
    {
      _id: 1,
      name: 'replica-set-mongo2-1:27018',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 1295,
      optime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeDurable: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeWritten: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeDate: ISODate('2024-11-18T00:13:50.000Z'),
      optimeDurableDate: ISODate('2024-11-18T00:13:50.000Z'),
      optimeWrittenDate: ISODate('2024-11-18T00:13:50.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastDurableWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastWrittenWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastHeartbeat: ISODate('2024-11-18T00:13:52.123Z'),
      lastHeartbeatRecv: ISODate('2024-11-18T00:13:51.995Z'),
      pingMs: Long('0'),
      lastHeartbeatMessage: '',
      syncSourceHost: 'replica-set-mongo1-1:27017',
      syncSourceId: 0,
      infoMessage: '',
      configVersion: 1,
      configTerm: 1
    },
    {
      _id: 2,
      name: 'replica-set-mongo3-1:27019',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 1295,
      optime: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeDurable: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeWritten: { ts: Timestamp({ t: 1731888830, i: 1 }), t: Long('1') },
      optimeDate: ISODate('2024-11-18T00:13:50.000Z'),
      optimeDurableDate: ISODate('2024-11-18T00:13:50.000Z'),
      optimeWrittenDate: ISODate('2024-11-18T00:13:50.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastDurableWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastWrittenWallTime: ISODate('2024-11-18T00:13:50.103Z'),
      lastHeartbeat: ISODate('2024-11-18T00:13:52.124Z'),
      lastHeartbeatRecv: ISODate('2024-11-18T00:13:53.506Z'),
      pingMs: Long('0'),
      lastHeartbeatMessage: '',
      syncSourceHost: 'replica-set-mongo1-1:27017',
      syncSourceId: 0,
      infoMessage: '',
      configVersion: 1,
      configTerm: 1
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731888830, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731888830, i: 1 })
}
```
- **Number of Nodes:** 3
- **Primary Node:** `replica-set-mongo1-1:27017`
- **Secondary Nodes:**
  - `replica-set-mongo2-1:27018`
  - `replica-set-mongo3-1:27019`
- **Node Health:** All nodes are healthy (`health: 1`).
- **Synchronization:** The primary node replicates all writes to the secondary nodes to maintain consistency.

## Step 6: Insert Data into the Primary

### Command:
```bash
rs0 [direct: primary] test> use testDB;
switched to db testDB
rs0 [direct: primary] testDB> db.users.insertOne({ name: "Alice", age: 25 });
{
  acknowledged: true,
  insertedId: ObjectId('673a86de7edd7744d4f7c614')
}
rs0 [direct: primary] testDB> db.users.find();
[
  { _id: ObjectId('673a86de7edd7744d4f7c614'), name: 'Alice', age: 25 }
]
```
## Step 7: Simulate a Primary Node Failure

### Command:
```bash
rs0 [direct: primary] test> rs.status();
{
  set: 'rs0',
  date: ISODate('2024-11-18T19:17:12.976Z'),
  myState: 1,
  term: Long('8'),
  syncSourceHost: '',
  syncSourceId: -1,
  heartbeatIntervalMillis: Long('2000'),
  majorityVoteCount: 2,
  writeMajorityCount: 2,
  votingMembersCount: 3,
  writableVotingMembersCount: 3,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
    lastCommittedWallTime: ISODate('2024-11-18T19:17:05.985Z'),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
    appliedOpTime: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
    durableOpTime: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
    writtenOpTime: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
    lastAppliedWallTime: ISODate('2024-11-18T19:17:05.985Z'),
    lastDurableWallTime: ISODate('2024-11-18T19:17:05.985Z'),
    lastWrittenWallTime: ISODate('2024-11-18T19:17:05.985Z')
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1731957395, i: 1 }),
  electionCandidateMetrics: {
    lastElectionReason: 'stepUpRequestSkipDryRun',
    lastElectionDate: ISODate('2024-11-18T19:16:25.951Z'),
    electionTerm: Long('8'),
    lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1731957385, i: 1 }), t: Long('7') },
    lastSeenWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731957385, i: 1 }), t: Long('7') },
    lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1731957385, i: 1 }), t: Long('7') },
    numVotesNeeded: 2,
    priorityAtElection: 1,
    electionTimeoutMillis: Long('10000'),
    priorPrimaryMemberId: 0,
    numCatchUpOps: Long('0'),
    newTermStartDate: ISODate('2024-11-18T19:16:25.965Z'),
    wMajorityWriteAvailabilityDate: ISODate('2024-11-18T19:16:25.981Z')
  },
  electionParticipantMetrics: {
    votedForCandidate: true,
    electionTerm: Long('7'),
    lastVoteDate: ISODate('2024-11-18T16:42:44.845Z'),
    electionCandidateMemberId: 0,
    voteReason: '',
    lastWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    maxWrittenOpTimeInSet: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    lastAppliedOpTimeAtElection: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    maxAppliedOpTimeInSet: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    priorityAtElection: 1
  },
  members: [
    {
      _id: 0,
      name: 'replica-set-mongo1-1:27017',
      health: 0,
      state: 8,
      stateStr: '(not reachable/healthy)',
      uptime: 0,
      optime: { ts: Timestamp({ t: 0, i: 0 }), t: Long('-1') },
      optimeDurable: { ts: Timestamp({ t: 0, i: 0 }), t: Long('-1') },
      optimeWritten: { ts: Timestamp({ t: 0, i: 0 }), t: Long('-1') },
      optimeDate: ISODate('1970-01-01T00:00:00.000Z'),
      optimeDurableDate: ISODate('1970-01-01T00:00:00.000Z'),
      optimeWrittenDate: ISODate('1970-01-01T00:00:00.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T19:16:35.970Z'),
      lastDurableWallTime: ISODate('2024-11-18T19:16:35.970Z'),
      lastWrittenWallTime: ISODate('2024-11-18T19:16:35.970Z'),
      lastHeartbeat: ISODate('2024-11-18T19:17:12.403Z'),
      lastHeartbeatRecv: ISODate('2024-11-18T19:16:34.490Z'),
      pingMs: Long('0'),
      lastHeartbeatMessage: 'Error connecting to replica-set-mongo1-1:27017 :: caused by :: Could not find address for replica-set-mongo1-1:27017: SocketException: onInvoke :: caused by :: Host not found (authoritative)',
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: '',
      configVersion: 1,
      configTerm: 8
    },
    {
      _id: 1,
      name: 'replica-set-mongo2-1:27018',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 75542,
      optime: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
      optimeDate: ISODate('2024-11-18T19:17:05.000Z'),
      optimeWritten: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
      optimeWrittenDate: ISODate('2024-11-18T19:17:05.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T19:17:05.985Z'),
      lastDurableWallTime: ISODate('2024-11-18T19:17:05.985Z'),
      lastWrittenWallTime: ISODate('2024-11-18T19:17:05.985Z'),
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: '',
      electionTime: Timestamp({ t: 1731957385, i: 2 }),
      electionDate: ISODate('2024-11-18T19:16:25.000Z'),
      configVersion: 1,
      configTerm: 8,
      self: true,
      lastHeartbeatMessage: ''
    },
    {
      _id: 2,
      name: 'replica-set-mongo3-1:27019',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 69894,
      optime: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
      optimeDurable: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
      optimeWritten: { ts: Timestamp({ t: 1731957425, i: 1 }), t: Long('8') },
      optimeDate: ISODate('2024-11-18T19:17:05.000Z'),
      optimeDurableDate: ISODate('2024-11-18T19:17:05.000Z'),
      optimeWrittenDate: ISODate('2024-11-18T19:17:05.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T19:17:05.985Z'),
      lastDurableWallTime: ISODate('2024-11-18T19:17:05.985Z'),
      lastWrittenWallTime: ISODate('2024-11-18T19:17:05.985Z'),
      lastHeartbeat: ISODate('2024-11-18T19:17:12.038Z'),
      lastHeartbeatRecv: ISODate('2024-11-18T19:17:12.679Z'),
      pingMs: Long('0'),
      lastHeartbeatMessage: '',
      syncSourceHost: 'replica-set-mongo2-1:27018',
      syncSourceId: 1,
      infoMessage: '',
      configVersion: 1,
      configTerm: 8
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731957425, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731957425, i: 1 })
}
```
- **Node `_id`:** 1 (`replica-set-mongo2-1:27018`) was elected as the new primary.
- **Health Status:**
  - `replica-set-mongo2-1:27018` (Primary): `health: 1` (healthy)
  - `replica-set-mongo3-1:27019` (Secondary): `health: 1` (healthy)
  - `replica-set-mongo1-1:27017` (Stopped): `health: 0` (not healthy)
- **Conclusion:** The two active nodes are healthy, while the stopped node is not.
- **Election Duration:** Approximately 41 seconds.

## Step 8: Insert Data into the New Primary

### Command:
```bash
rs0 [direct: primary] test> use testDB;
switched to db testDB
rs0 [direct: primary] testDB> db.users.insertOne({ name: "Bob", age: 30 });
{
  acknowledged: true,
  insertedId: ObjectId('673b9485845bb1b0adf7c614')
}
rs0 [direct: primary] testDB> db.users.find();
[
  { _id: ObjectId('673b91359949466212f7c614'), name: 'Alice', age: 25 },
  { _id: ObjectId('673b9485845bb1b0adf7c614'), name: 'Bob', age: 30 }
]
```

## Step 9: Restart the Stopped Node
### Command:
```bash
rs0 [direct: primary] test> rs.status();
{
  set: 'rs0',
  date: ISODate('2024-11-18T19:28:16.075Z'),
  myState: 1,
  term: Long('8'),
  syncSourceHost: '',
  syncSourceId: -1,
  heartbeatIntervalMillis: Long('2000'),
  majorityVoteCount: 2,
  writeMajorityCount: 2,
  votingMembersCount: 3,
  writableVotingMembersCount: 3,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
    lastCommittedWallTime: ISODate('2024-11-18T19:28:06.145Z'),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
    appliedOpTime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
    durableOpTime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
    writtenOpTime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
    lastAppliedWallTime: ISODate('2024-11-18T19:28:06.145Z'),
    lastDurableWallTime: ISODate('2024-11-18T19:28:06.145Z'),
    lastWrittenWallTime: ISODate('2024-11-18T19:28:06.145Z')
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1731958056, i: 1 }),
  electionCandidateMetrics: {
    lastElectionReason: 'stepUpRequestSkipDryRun',
    lastElectionDate: ISODate('2024-11-18T19:16:25.951Z'),
    electionTerm: Long('8'),
    lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1731957385, i: 1 }), t: Long('7') },
    lastSeenWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731957385, i: 1 }), t: Long('7') },
    lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1731957385, i: 1 }), t: Long('7') },
    numVotesNeeded: 2,
    priorityAtElection: 1,
    electionTimeoutMillis: Long('10000'),
    priorPrimaryMemberId: 0,
    numCatchUpOps: Long('0'),
    newTermStartDate: ISODate('2024-11-18T19:16:25.965Z'),
    wMajorityWriteAvailabilityDate: ISODate('2024-11-18T19:16:25.981Z')
  },
  electionParticipantMetrics: {
    votedForCandidate: true,
    electionTerm: Long('7'),
    lastVoteDate: ISODate('2024-11-18T16:42:44.845Z'),
    electionCandidateMemberId: 0,
    voteReason: '',
    lastWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    maxWrittenOpTimeInSet: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    lastAppliedOpTimeAtElection: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    maxAppliedOpTimeInSet: { ts: Timestamp({ t: 1731943399, i: 1 }), t: Long('6') },
    priorityAtElection: 1
  },
  members: [
    {
      _id: 0,
      name: 'replica-set-mongo1-1:27017',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 116,
      optime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeDurable: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeWritten: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeDate: ISODate('2024-11-18T19:28:06.000Z'),
      optimeDurableDate: ISODate('2024-11-18T19:28:06.000Z'),
      optimeWrittenDate: ISODate('2024-11-18T19:28:06.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastDurableWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastWrittenWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastHeartbeat: ISODate('2024-11-18T19:28:15.913Z'),
      lastHeartbeatRecv: ISODate('2024-11-18T19:28:15.110Z'),
      pingMs: Long('0'),
      lastHeartbeatMessage: '',
      syncSourceHost: 'replica-set-mongo3-1:27019',
      syncSourceId: 2,
      infoMessage: '',
      configVersion: 1,
      configTerm: 8
    },
    {
      _id: 1,
      name: 'replica-set-mongo2-1:27018',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 76206,
      optime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeDate: ISODate('2024-11-18T19:28:06.000Z'),
      optimeWritten: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeWrittenDate: ISODate('2024-11-18T19:28:06.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastDurableWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastWrittenWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: '',
      electionTime: Timestamp({ t: 1731957385, i: 2 }),
      electionDate: ISODate('2024-11-18T19:16:25.000Z'),
      configVersion: 1,
      configTerm: 8,
      self: true,
      lastHeartbeatMessage: ''
    },
    {
      _id: 2,
      name: 'replica-set-mongo3-1:27019',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 70557,
      optime: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeDurable: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeWritten: { ts: Timestamp({ t: 1731958086, i: 1 }), t: Long('8') },
      optimeDate: ISODate('2024-11-18T19:28:06.000Z'),
      optimeDurableDate: ISODate('2024-11-18T19:28:06.000Z'),
      optimeWrittenDate: ISODate('2024-11-18T19:28:06.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastDurableWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastWrittenWallTime: ISODate('2024-11-18T19:28:06.145Z'),
      lastHeartbeat: ISODate('2024-11-18T19:28:15.567Z'),
      lastHeartbeatRecv: ISODate('2024-11-18T19:28:15.964Z'),
      pingMs: Long('0'),
      lastHeartbeatMessage: '',
      syncSourceHost: 'replica-set-mongo2-1:27018',
      syncSourceId: 1,
      infoMessage: '',
      configVersion: 1,
      configTerm: 8
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731958086, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731958086, i: 1 })
}
```
- **Restarted Node:** `replica-set-mongo1-1:27017` has successfully rejoined the replica set as a **SECONDARY** node.
- **Health Status:**
  - `replica-set-mongo1-1` (restarted): `"health": 1` (healthy)
  - `replica-set-mongo2-1` (current primary): `"health": 1` (healthy)
  - `replica-set-mongo3-1` (secondary): `"health": 1` (healthy)
- **Conclusion:** All nodes in the replica set are healthy.

MongoDB ensured consistency through:
1. **Operations Log:** Replaying missed operations to the restarted node.
2. **Synchronization:** The restarted node synchronized with the sync source (`replica-set-mongo3-1`) until it caught up with the current state.
3. **Seamless Integration:** After synchronization, the restarted node successfully integrated as a **SECONDARY**.

