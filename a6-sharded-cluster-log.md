## Step 2: Start the Sharded Cluster
### Command:
```bash
(base) chenxizhao@zhaochenxideMacBook-Pro sharded-cluster % docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                 NAMES
30c0c36216eb   mongo:latest   "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   0.0.0.0:27017->27017/tcp              sharded-cluster-mongos-1
95d3e59f09ff   mongo:latest   "docker-entrypoint.s…"   8 seconds ago   Up 8 seconds   27017/tcp, 0.0.0.0:27032->27032/tcp   sharded-cluster-shard2-1
a2c9bdaf3d93   mongo:latest   "docker-entrypoint.s…"   8 seconds ago   Up 8 seconds   27017/tcp, 0.0.0.0:27031->27031/tcp   sharded-cluster-shard1-1
059bf9c91f30   mongo:latest   "docker-entrypoint.s…"   8 seconds ago   Up 8 seconds   27017/tcp, 0.0.0.0:27021->27021/tcp   sharded-cluster-config1-1
```
### Explanation of Running Containers:
- **`sharded-cluster-mongos-1`:** Acts as the query router. It directs client queries to the appropriate shard.
- **`sharded-cluster-shard1-1` & `sharded-cluster-shard2-1`:** These are the two shards that store distributed portions of the database.
- **`sharded-cluster-config1-1`:** Manages the metadata and configuration for the cluster, ensuring proper routing and coordination of queries.

## Step 3: Initialize the Config Server Replica Set
### Command:
```bash
configReplSet [direct: secondary] test> rs.status();
{
  set: 'configReplSet',
  date: ISODate('2024-11-18T20:28:11.825Z'),
  myState: 1,
  term: Long('1'),
  syncSourceHost: '',
  syncSourceId: -1,
  configsvr: true,
  heartbeatIntervalMillis: Long('2000'),
  majorityVoteCount: 1,
  writeMajorityCount: 1,
  votingMembersCount: 1,
  writableVotingMembersCount: 1,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1731961691, i: 1 }), t: Long('1') },
    lastCommittedWallTime: ISODate('2024-11-18T20:28:11.100Z'),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1731961691, i: 1 }), t: Long('1') },
    appliedOpTime: { ts: Timestamp({ t: 1731961691, i: 1 }), t: Long('1') },
    durableOpTime: { ts: Timestamp({ t: 1731961691, i: 1 }), t: Long('1') },
    writtenOpTime: { ts: Timestamp({ t: 1731961691, i: 1 }), t: Long('1') },
    lastAppliedWallTime: ISODate('2024-11-18T20:28:11.100Z'),
    lastDurableWallTime: ISODate('2024-11-18T20:28:11.100Z'),
    lastWrittenWallTime: ISODate('2024-11-18T20:28:11.100Z')
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1731961674, i: 1 }),
  electionCandidateMetrics: {
    lastElectionReason: 'electionTimeout',
    lastElectionDate: ISODate('2024-11-18T20:27:54.928Z'),
    electionTerm: Long('1'),
    lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1731961674, i: 1 }), t: Long('-1') },
    lastSeenWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731961674, i: 1 }), t: Long('-1') },
    lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1731961674, i: 1 }), t: Long('-1') },
    numVotesNeeded: 1,
    priorityAtElection: 1,
    electionTimeoutMillis: Long('10000'),
    newTermStartDate: ISODate('2024-11-18T20:27:54.964Z'),
    wMajorityWriteAvailabilityDate: ISODate('2024-11-18T20:27:55.073Z')
  },
  members: [
    {
      _id: 0,
      name: 'host.docker.internal:27021',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 402,
      optime: { ts: Timestamp({ t: 1731961691, i: 1 }), t: Long('1') },
      optimeDate: ISODate('2024-11-18T20:28:11.000Z'),
      optimeWritten: { ts: Timestamp({ t: 1731961691, i: 1 }), t: Long('1') },
      optimeWrittenDate: ISODate('2024-11-18T20:28:11.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T20:28:11.100Z'),
      lastDurableWallTime: ISODate('2024-11-18T20:28:11.100Z'),
      lastWrittenWallTime: ISODate('2024-11-18T20:28:11.100Z'),
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: 'Could not find member to sync from',
      electionTime: Timestamp({ t: 1731961674, i: 2 }),
      electionDate: ISODate('2024-11-18T20:27:54.000Z'),
      configVersion: 1,
      configTerm: 1,
      self: true,
      lastHeartbeatMessage: ''
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731961691, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731961691, i: 1 })
}
```
## Step 4: Initialize the Shard Replica Sets
### Command:
```bash
shard1ReplSet [direct: secondary] test> rs.status();
{
  set: 'shard1ReplSet',
  date: ISODate('2024-11-18T20:30:44.150Z'),
  myState: 1,
  term: Long('1'),
  syncSourceHost: '',
  syncSourceId: -1,
  heartbeatIntervalMillis: Long('2000'),
  majorityVoteCount: 1,
  writeMajorityCount: 1,
  votingMembersCount: 1,
  writableVotingMembersCount: 1,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1731961837, i: 16 }), t: Long('1') },
    lastCommittedWallTime: ISODate('2024-11-18T20:30:37.504Z'),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1731961837, i: 16 }), t: Long('1') },
    appliedOpTime: { ts: Timestamp({ t: 1731961837, i: 16 }), t: Long('1') },
    durableOpTime: { ts: Timestamp({ t: 1731961837, i: 16 }), t: Long('1') },
    writtenOpTime: { ts: Timestamp({ t: 1731961837, i: 16 }), t: Long('1') },
    lastAppliedWallTime: ISODate('2024-11-18T20:30:37.504Z'),
    lastDurableWallTime: ISODate('2024-11-18T20:30:37.504Z'),
    lastWrittenWallTime: ISODate('2024-11-18T20:30:37.504Z')
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1731961837, i: 1 }),
  electionCandidateMetrics: {
    lastElectionReason: 'electionTimeout',
    lastElectionDate: ISODate('2024-11-18T20:30:37.426Z'),
    electionTerm: Long('1'),
    lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1731961837, i: 1 }), t: Long('-1') },
    lastSeenWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731961837, i: 1 }), t: Long('-1') },
    lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1731961837, i: 1 }), t: Long('-1') },
    numVotesNeeded: 1,
    priorityAtElection: 1,
    electionTimeoutMillis: Long('10000'),
    newTermStartDate: ISODate('2024-11-18T20:30:37.452Z'),
    wMajorityWriteAvailabilityDate: ISODate('2024-11-18T20:30:37.477Z')
  },
  members: [
    {
      _id: 0,
      name: 'host.docker.internal:27031',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 555,
      optime: { ts: Timestamp({ t: 1731961837, i: 16 }), t: Long('1') },
      optimeDate: ISODate('2024-11-18T20:30:37.000Z'),
      optimeWritten: { ts: Timestamp({ t: 1731961837, i: 16 }), t: Long('1') },
      optimeWrittenDate: ISODate('2024-11-18T20:30:37.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T20:30:37.504Z'),
      lastDurableWallTime: ISODate('2024-11-18T20:30:37.504Z'),
      lastWrittenWallTime: ISODate('2024-11-18T20:30:37.504Z'),
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: 'Could not find member to sync from',
      electionTime: Timestamp({ t: 1731961837, i: 2 }),
      electionDate: ISODate('2024-11-18T20:30:37.000Z'),
      configVersion: 1,
      configTerm: 1,
      self: true,
      lastHeartbeatMessage: ''
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731961837, i: 16 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731961837, i: 16 })
}
```
```bash
shard2ReplSet [direct: other] test> rs.status();
{
  set: 'shard2ReplSet',
  date: ISODate('2024-11-18T20:31:51.087Z'),
  myState: 1,
  term: Long('1'),
  syncSourceHost: '',
  syncSourceId: -1,
  heartbeatIntervalMillis: Long('2000'),
  majorityVoteCount: 1,
  writeMajorityCount: 1,
  votingMembersCount: 1,
  writableVotingMembersCount: 1,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1731961903, i: 16 }), t: Long('1') },
    lastCommittedWallTime: ISODate('2024-11-18T20:31:43.600Z'),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1731961903, i: 16 }), t: Long('1') },
    appliedOpTime: { ts: Timestamp({ t: 1731961903, i: 16 }), t: Long('1') },
    durableOpTime: { ts: Timestamp({ t: 1731961903, i: 16 }), t: Long('1') },
    writtenOpTime: { ts: Timestamp({ t: 1731961903, i: 16 }), t: Long('1') },
    lastAppliedWallTime: ISODate('2024-11-18T20:31:43.600Z'),
    lastDurableWallTime: ISODate('2024-11-18T20:31:43.600Z'),
    lastWrittenWallTime: ISODate('2024-11-18T20:31:43.600Z')
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1731961903, i: 1 }),
  electionCandidateMetrics: {
    lastElectionReason: 'electionTimeout',
    lastElectionDate: ISODate('2024-11-18T20:31:43.524Z'),
    electionTerm: Long('1'),
    lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1731961903, i: 1 }), t: Long('-1') },
    lastSeenWrittenOpTimeAtElection: { ts: Timestamp({ t: 1731961903, i: 1 }), t: Long('-1') },
    lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1731961903, i: 1 }), t: Long('-1') },
    numVotesNeeded: 1,
    priorityAtElection: 1,
    electionTimeoutMillis: Long('10000'),
    newTermStartDate: ISODate('2024-11-18T20:31:43.547Z'),
    wMajorityWriteAvailabilityDate: ISODate('2024-11-18T20:31:43.568Z')
  },
  members: [
    {
      _id: 0,
      name: 'host.docker.internal:27032',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 622,
      optime: { ts: Timestamp({ t: 1731961903, i: 16 }), t: Long('1') },
      optimeDate: ISODate('2024-11-18T20:31:43.000Z'),
      optimeWritten: { ts: Timestamp({ t: 1731961903, i: 16 }), t: Long('1') },
      optimeWrittenDate: ISODate('2024-11-18T20:31:43.000Z'),
      lastAppliedWallTime: ISODate('2024-11-18T20:31:43.600Z'),
      lastDurableWallTime: ISODate('2024-11-18T20:31:43.600Z'),
      lastWrittenWallTime: ISODate('2024-11-18T20:31:43.600Z'),
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: 'Could not find member to sync from',
      electionTime: Timestamp({ t: 1731961903, i: 2 }),
      electionDate: ISODate('2024-11-18T20:31:43.000Z'),
      configVersion: 1,
      configTerm: 1,
      self: true,
      lastHeartbeatMessage: ''
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731961903, i: 16 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731961903, i: 16 })
}
```
## Step 5: Add Shards to the Cluster
### Command:
```bash
[direct: mongos] test> sh.status();
shardingVersion
{ _id: 1, clusterId: ObjectId('673ba34bac9c4b15394b616d') }
---
shards
[
  {
    _id: 'shard1ReplSet',
    host: 'shard1ReplSet/host.docker.internal:27031',
    state: 1,
    topologyTime: Timestamp({ t: 1731961994, i: 10 }),
    replSetConfigVersion: Long('-1')
  },
  {
    _id: 'shard2ReplSet',
    host: 'shard2ReplSet/host.docker.internal:27032',
    state: 1,
    topologyTime: Timestamp({ t: 1731962001, i: 9 }),
    replSetConfigVersion: Long('-1')
  }
]
---
active mongoses
[ { '8.0.3': 1 } ]
---
autosplit
{ 'Currently enabled': 'yes' }
---
balancer
{
  'Currently enabled': 'yes',
  'Currently running': 'no',
  'Failed balancer rounds in last 5 attempts': 0,
  'Migration Results for the last 24 hours': 'No recent migrations'
}
---
shardedDataDistribution
[]
---
databases
[
  {
    database: { _id: 'config', primary: 'config', partitioned: true },
    collections: {}
  }
]
```
#### 1. How many shards are in the cluster?
- The cluster has **two shards**, as specified during the `sh.addShard()` commands:
  - `shard1ReplSet/host.docker.internal:27031`
  - `shard2ReplSet/host.docker.internal:27032`
- Shards are distributed portions of the database, enabling horizontal scaling of the data.

#### 2. How many mongos routers are running?
- There is **one mongos router**, which acts as the query router and coordinates client requests to the appropriate shards.

#### 3. Is autosplit enabled? What does it mean?
- The `sh.status()` output typically includes a field indicating whether autosplit is enabled.
- If enabled, **autosplit** automatically splits large chunks of data into smaller chunks when they exceed the configured chunk size. This helps maintain an even distribution of data across shards.

#### 4. Is the balancer enabled? What does it do?
- The `sh.status()` output also specifies whether the balancer is enabled.
- If enabled, the **balancer** redistributes chunks of data across shards to ensure they are evenly balanced. This prevents any single shard from becoming a bottleneck due to disproportionate data loads.

## Step 6: Enable Sharding on a Database
### Command:
```bash
[direct: mongos] test> sh.status();
shardingVersion
{ _id: 1, clusterId: ObjectId('673ba34bac9c4b15394b616d') }
---
shards
[
  {
    _id: 'shard1ReplSet',
    host: 'shard1ReplSet/host.docker.internal:27031',
    state: 1,
    topologyTime: Timestamp({ t: 1731961994, i: 10 }),
    replSetConfigVersion: Long('-1')
  },
  {
    _id: 'shard2ReplSet',
    host: 'shard2ReplSet/host.docker.internal:27032',
    state: 1,
    topologyTime: Timestamp({ t: 1731962001, i: 9 }),
    replSetConfigVersion: Long('-1')
  }
]
---
active mongoses
[ { '8.0.3': 1 } ]
---
autosplit
{ 'Currently enabled': 'yes' }
---
balancer
{
  'Currently enabled': 'yes',
  'Failed balancer rounds in last 5 attempts': 0,
  'Currently running': 'no',
  'Migration Results for the last 24 hours': 'No recent migrations'
}
---
shardedDataDistribution
[
  {
    ns: 'config.system.sessions',
    shards: [
      {
        shardName: 'shard1ReplSet',
        numOrphanedDocs: 0,
        numOwnedDocuments: 6,
        ownedSizeBytes: 594,
        orphanedSizeBytes: 0
      }
    ]
  },
  {
    ns: 'testDB.users',
    shards: [
      {
        shardName: 'shard1ReplSet',
        numOrphanedDocs: 0,
        numOwnedDocuments: 0,
        ownedSizeBytes: 0,
        orphanedSizeBytes: 0
      },
      {
        shardName: 'shard2ReplSet',
        numOrphanedDocs: 0,
        numOwnedDocuments: 0,
        ownedSizeBytes: 0,
        orphanedSizeBytes: 0
      }
    ]
  }
]
---
databases
[
  {
    database: { _id: 'config', primary: 'config', partitioned: true },
    collections: {
      'config.system.sessions': {
        shardKey: { _id: 1 },
        unique: false,
        balancing: true,
        chunkMetadata: [ { shard: 'shard1ReplSet', nChunks: 1 } ],
        chunks: [
          { min: { _id: MinKey() }, max: { _id: MaxKey() }, 'on shard': 'shard1ReplSet', 'last modified': Timestamp({ t: 1, i: 0 }) }
        ],
        tags: []
      }
    }
  },
  {
    database: {
      _id: 'testDB',
      primary: 'shard2ReplSet',
      version: {
        uuid: UUID('bcc98b8c-0e5b-4b83-992a-701a910d0dc7'),
        timestamp: Timestamp({ t: 1731962464, i: 2 }),
        lastMod: 1
      }
    },
    collections: {
      'testDB.users': {
        shardKey: { name: 'hashed' },
        unique: false,
        balancing: true,
        chunkMetadata: [
          { shard: 'shard1ReplSet', nChunks: 1 },
          { shard: 'shard2ReplSet', nChunks: 1 }
        ],
        chunks: [
          { min: { name: MinKey() }, max: { name: Long('0') }, 'on shard': 'shard2ReplSet', 'last modified': Timestamp({ t: 1, i: 0 }) },
          { min: { name: Long('0') }, max: { name: MaxKey() }, 'on shard': 'shard1ReplSet', 'last modified': Timestamp({ t: 1, i: 1 }) }
        ],
        tags: []
      }
    }
  }
]
```
## Step 8: Check Document Distribution
### Command:
```bash
[direct: mongos] testDB> db.users.getShardDistribution()
Shard shard1ReplSet at shard1ReplSet/host.docker.internal:27031
{
  data: '23KiB',
  docs: 485,
  chunks: 1,
  'estimated data per chunk': '23KiB',
  'estimated docs per chunk': 485
}
---
Shard shard2ReplSet at shard2ReplSet/host.docker.internal:27032
{
  data: '24KiB',
  docs: 515,
  chunks: 1,
  'estimated data per chunk': '24KiB',
  'estimated docs per chunk': 515
}
---
Totals
{
  data: '47KiB',
  docs: 1000,
  chunks: 2,
  'Shard shard1ReplSet': [
    '48.5 % data',
    '48.5 % docs in cluster',
    '48B avg obj size on shard'
  ],
  'Shard shard2ReplSet': [
    '51.49 % data',
    '51.5 % docs in cluster',
    '48B avg obj size on shard'
  ]
}
```
#### 1. How are the documents distributed across shards? How can you tell?
- **Document Distribution**:
  - Shard `shard1ReplSet` contains 485 documents (48.5% of the total documents).
  - Shard `shard2ReplSet` contains 515 documents (51.5% of the total documents).
- **Observation**:
  - The `db.users.getShardDistribution()` output provides the exact number of documents and the percentage of the total documents in each shard.
  - The distribution is close to even but not exactly 50-50 due to the randomness of the hash-based sharding.

#### 2. Are the chunks balanced across shards? How can you tell?
- **Chunk Balance**:
  - Each shard has **1 chunk**, as indicated by the `chunks: 1` field for both shards.
- **Observation**:
  - The `db.users.getShardDistribution()` output shows that the chunks are evenly distributed, with one chunk on each shard. 
  - Although the number of documents and data size are slightly different, this is acceptable as the balancer works to ensure even distribution over time.

## Step 9: Query the Sharded Collection
### User 100 is from shard1, and User 150 is from shard2.
### Command:
```bash
[direct: mongos] testDB> db.users.find({ name: "User100" }).explain("executionStats");
{
  queryPlanner: {
    winningPlan: {
      stage: 'SINGLE_SHARD',
      shards: [
        {
          explainVersion: '1',
          shardName: 'shard1ReplSet',
          connectionString: 'shard1ReplSet/host.docker.internal:27031',
          serverInfo: {
            host: 'a2c9bdaf3d93',
            port: 27031,
            version: '8.0.3',
            gitVersion: '89d97f2744a2b9851ddfb51bdf22f687562d9b06'
          },
          namespace: 'testDB.users',
          parsedQuery: { name: { '$eq': 'User100' } },
          indexFilterSet: false,
          queryHash: '544F3E5C',
          planCacheKey: 'EEE0759C',
          optimizationTimeMillis: 0,
          maxIndexedOrSolutionsReached: false,
          maxIndexedAndSolutionsReached: false,
          maxScansToExplodeReached: false,
          prunedSimilarIndexes: false,
          winningPlan: {
            isCached: false,
            stage: 'FETCH',
            filter: { name: { '$eq': 'User100' } },
            inputStage: {
              stage: 'IXSCAN',
              keyPattern: { name: 'hashed' },
              indexName: 'name_hashed',
              isMultiKey: false,
              isUnique: false,
              isSparse: false,
              isPartial: false,
              indexVersion: 2,
              direction: 'forward',
              indexBounds: {
                name: [ '[8593700602177999620, 8593700602177999620]' ]
              }
            }
          },
          rejectedPlans: []
        }
      ]
    }
  },
  executionStats: {
    nReturned: 1,
    executionTimeMillis: 1,
    totalKeysExamined: 1,
    totalDocsExamined: 1,
    executionStages: {
      stage: 'SINGLE_SHARD',
      nReturned: 1,
      executionTimeMillis: 1,
      totalKeysExamined: 1,
      totalDocsExamined: 1,
      totalChildMillis: Long('0'),
      shards: [
        {
          shardName: 'shard1ReplSet',
          executionSuccess: true,
          nReturned: 1,
          executionTimeMillis: 0,
          totalKeysExamined: 1,
          totalDocsExamined: 1,
          executionStages: {
            isCached: false,
            stage: 'FETCH',
            filter: { name: { '$eq': 'User100' } },
            nReturned: 1,
            executionTimeMillisEstimate: 0,
            works: 2,
            advanced: 1,
            needTime: 0,
            needYield: 0,
            saveState: 0,
            restoreState: 0,
            isEOF: 1,
            docsExamined: 1,
            alreadyHasObj: 0,
            inputStage: {
              stage: 'IXSCAN',
              nReturned: 1,
              executionTimeMillisEstimate: 0,
              works: 2,
              advanced: 1,
              needTime: 0,
              needYield: 0,
              saveState: 0,
              restoreState: 0,
              isEOF: 1,
              keyPattern: { name: 'hashed' },
              indexName: 'name_hashed',
              isMultiKey: false,
              isUnique: false,
              isSparse: false,
              isPartial: false,
              indexVersion: 2,
              direction: 'forward',
              indexBounds: {
                name: [ '[8593700602177999620, 8593700602177999620]' ]
              },
              keysExamined: 1,
              seeks: 1,
              dupsTested: 0,
              dupsDropped: 0
            }
          }
        }
      ]
    }
  },
  serverInfo: {
    host: '30c0c36216eb',
    port: 27017,
    version: '8.0.3',
    gitVersion: '89d97f2744a2b9851ddfb51bdf22f687562d9b06'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted',
    internalQueryPlannerIgnoreIndexWithCollationForRegex: 1
  },
  command: {
    find: 'users',
    filter: { name: 'User100' },
    lsid: { id: UUID('a8c0deec-e08b-4c25-be9f-c3691dcf1736') },
    '$clusterTime': {
      clusterTime: Timestamp({ t: 1731963581, i: 1 }),
      signature: {
        hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
        keyId: 0
      }
    },
    '$db': 'testDB'
  },
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731963581, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731963575, i: 1 })
}
```
```bash
[direct: mongos] testDB> db.users.find({ name: "User150" }).explain("executionStats");
{
  queryPlanner: {
    winningPlan: {
      stage: 'SINGLE_SHARD',
      shards: [
        {
          explainVersion: '1',
          shardName: 'shard2ReplSet',
          connectionString: 'shard2ReplSet/host.docker.internal:27032',
          serverInfo: {
            host: '95d3e59f09ff',
            port: 27032,
            version: '8.0.3',
            gitVersion: '89d97f2744a2b9851ddfb51bdf22f687562d9b06'
          },
          namespace: 'testDB.users',
          parsedQuery: { name: { '$eq': 'User150' } },
          indexFilterSet: false,
          queryHash: '544F3E5C',
          planCacheKey: 'EEE0759C',
          optimizationTimeMillis: 0,
          maxIndexedOrSolutionsReached: false,
          maxIndexedAndSolutionsReached: false,
          maxScansToExplodeReached: false,
          prunedSimilarIndexes: false,
          winningPlan: {
            isCached: false,
            stage: 'FETCH',
            filter: { name: { '$eq': 'User150' } },
            inputStage: {
              stage: 'IXSCAN',
              keyPattern: { name: 'hashed' },
              indexName: 'name_hashed',
              isMultiKey: false,
              isUnique: false,
              isSparse: false,
              isPartial: false,
              indexVersion: 2,
              direction: 'forward',
              indexBounds: {
                name: [ '[-7279619628963499949, -7279619628963499949]' ]
              }
            }
          },
          rejectedPlans: []
        }
      ]
    }
  },
  executionStats: {
    nReturned: 1,
    executionTimeMillis: 6,
    totalKeysExamined: 1,
    totalDocsExamined: 1,
    executionStages: {
      stage: 'SINGLE_SHARD',
      nReturned: 1,
      executionTimeMillis: 6,
      totalKeysExamined: 1,
      totalDocsExamined: 1,
      totalChildMillis: Long('0'),
      shards: [
        {
          shardName: 'shard2ReplSet',
          executionSuccess: true,
          nReturned: 1,
          executionTimeMillis: 0,
          totalKeysExamined: 1,
          totalDocsExamined: 1,
          executionStages: {
            isCached: false,
            stage: 'FETCH',
            filter: { name: { '$eq': 'User150' } },
            nReturned: 1,
            executionTimeMillisEstimate: 0,
            works: 2,
            advanced: 1,
            needTime: 0,
            needYield: 0,
            saveState: 0,
            restoreState: 0,
            isEOF: 1,
            docsExamined: 1,
            alreadyHasObj: 0,
            inputStage: {
              stage: 'IXSCAN',
              nReturned: 1,
              executionTimeMillisEstimate: 0,
              works: 2,
              advanced: 1,
              needTime: 0,
              needYield: 0,
              saveState: 0,
              restoreState: 0,
              isEOF: 1,
              keyPattern: { name: 'hashed' },
              indexName: 'name_hashed',
              isMultiKey: false,
              isUnique: false,
              isSparse: false,
              isPartial: false,
              indexVersion: 2,
              direction: 'forward',
              indexBounds: {
                name: [ '[-7279619628963499949, -7279619628963499949]' ]
              },
              keysExamined: 1,
              seeks: 1,
              dupsTested: 0,
              dupsDropped: 0
            }
          }
        }
      ]
    }
  },
  serverInfo: {
    host: '30c0c36216eb',
    port: 27017,
    version: '8.0.3',
    gitVersion: '89d97f2744a2b9851ddfb51bdf22f687562d9b06'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted',
    internalQueryPlannerIgnoreIndexWithCollationForRegex: 1
  },
  command: {
    find: 'users',
    filter: { name: 'User150' },
    lsid: { id: UUID('a8c0deec-e08b-4c25-be9f-c3691dcf1736') },
    '$clusterTime': {
      clusterTime: Timestamp({ t: 1731963581, i: 1 }),
      signature: {
        hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
        keyId: 0
      }
    },
    '$db': 'testDB'
  },
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731964959, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731964951, i: 1 })
}
```
## Step 10: Simulate a Shard Failure
### Command:
```bash
[direct: mongos] testDB> db.users.find({ name: "User100" }).explain("executionStats")
MongoServerError[FailedToSatisfyReadPreference]: Explain command on shard shard1ReplSet failed :: caused by :: Could not find host matching read preference { mode: "primary" } for set shard1ReplSet
```
```bash
[direct: mongos] testDB> db.users.find({ name: "User150" }).explain("executionStats")
{
  queryPlanner: {
    winningPlan: {
      stage: 'SINGLE_SHARD',
      shards: [
        {
          explainVersion: '1',
          shardName: 'shard2ReplSet',
          connectionString: 'shard2ReplSet/host.docker.internal:27032',
          serverInfo: {
            host: '95d3e59f09ff',
            port: 27032,
            version: '8.0.3',
            gitVersion: '89d97f2744a2b9851ddfb51bdf22f687562d9b06'
          },
          namespace: 'testDB.users',
          parsedQuery: { name: { '$eq': 'User150' } },
          indexFilterSet: false,
          queryHash: '544F3E5C',
          planCacheKey: 'EEE0759C',
          optimizationTimeMillis: 0,
          maxIndexedOrSolutionsReached: false,
          maxIndexedAndSolutionsReached: false,
          maxScansToExplodeReached: false,
          prunedSimilarIndexes: false,
          winningPlan: {
            isCached: false,
            stage: 'FETCH',
            filter: { name: { '$eq': 'User150' } },
            inputStage: {
              stage: 'IXSCAN',
              keyPattern: { name: 'hashed' },
              indexName: 'name_hashed',
              isMultiKey: false,
              isUnique: false,
              isSparse: false,
              isPartial: false,
              indexVersion: 2,
              direction: 'forward',
              indexBounds: {
                name: [ '[-7279619628963499949, -7279619628963499949]' ]
              }
            }
          },
          rejectedPlans: []
        }
      ]
    }
  },
  executionStats: {
    nReturned: 1,
    executionTimeMillis: 2,
    totalKeysExamined: 1,
    totalDocsExamined: 1,
    executionStages: {
      stage: 'SINGLE_SHARD',
      nReturned: 1,
      executionTimeMillis: 2,
      totalKeysExamined: 1,
      totalDocsExamined: 1,
      totalChildMillis: Long('0'),
      shards: [
        {
          shardName: 'shard2ReplSet',
          executionSuccess: true,
          nReturned: 1,
          executionTimeMillis: 0,
          totalKeysExamined: 1,
          totalDocsExamined: 1,
          executionStages: {
            isCached: false,
            stage: 'FETCH',
            filter: { name: { '$eq': 'User150' } },
            nReturned: 1,
            executionTimeMillisEstimate: 0,
            works: 2,
            advanced: 1,
            needTime: 0,
            needYield: 0,
            saveState: 0,
            restoreState: 0,
            isEOF: 1,
            docsExamined: 1,
            alreadyHasObj: 0,
            inputStage: {
              stage: 'IXSCAN',
              nReturned: 1,
              executionTimeMillisEstimate: 0,
              works: 2,
              advanced: 1,
              needTime: 0,
              needYield: 0,
              saveState: 0,
              restoreState: 0,
              isEOF: 1,
              keyPattern: { name: 'hashed' },
              indexName: 'name_hashed',
              isMultiKey: false,
              isUnique: false,
              isSparse: false,
              isPartial: false,
              indexVersion: 2,
              direction: 'forward',
              indexBounds: {
                name: [ '[-7279619628963499949, -7279619628963499949]' ]
              },
              keysExamined: 1,
              seeks: 1,
              dupsTested: 0,
              dupsDropped: 0
            }
          }
        }
      ]
    }
  },
  serverInfo: {
    host: '30c0c36216eb',
    port: 27017,
    version: '8.0.3',
    gitVersion: '89d97f2744a2b9851ddfb51bdf22f687562d9b06'
  },
  serverParameters: {
    internalQueryFacetBufferSizeBytes: 104857600,
    internalQueryFacetMaxOutputDocSizeBytes: 104857600,
    internalLookupStageIntermediateDocumentMaxSizeBytes: 104857600,
    internalDocumentSourceGroupMaxMemoryBytes: 104857600,
    internalQueryMaxBlockingSortMemoryUsageBytes: 104857600,
    internalQueryProhibitBlockingMergeOnMongoS: 0,
    internalQueryMaxAddToSetBytes: 104857600,
    internalDocumentSourceSetWindowFieldsMaxMemoryBytes: 104857600,
    internalQueryFrameworkControl: 'trySbeRestricted',
    internalQueryPlannerIgnoreIndexWithCollationForRegex: 1
  },
  command: {
    find: 'users',
    filter: { name: 'User150' },
    lsid: { id: UUID('06e7b106-112f-4c07-9d0a-ac84cd29c05e') },
    '$clusterTime': {
      clusterTime: Timestamp({ t: 1731967982, i: 1 }),
      signature: {
        hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
        keyId: 0
      }
    },
    '$db': 'testDB'
  },
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1731967986, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('AAAAAAAAAAAAAAAAAAAAAAAAAAA=', 0),
      keyId: Long('0')
    }
  },
  operationTime: Timestamp({ t: 1731967982, i: 1 })
}
```
#### What do you observe in the `sh.status()` output?

- The metadata still lists both shards as active (`state: 1`).
- The `testDB.users` collection is distributed across two chunks, one on each shard.
- The balancer is enabled but not running, so no data migrations are occurring.

#### What happens when you query for a document on the stopped shard?

- The query fails with an error indicating the shard cannot satisfy the read preference because it is unavailable.

#### What happens when you query for a document on the other shard?

- The query succeeds, retrieving the document from the active shard (`shard2ReplSet`).

## Step 11: Restart the Stopped Shard
### Command:
```bash
[direct: mongos] testDB> sh.status();
shardingVersion
{ _id: 1, clusterId: ObjectId('673ba34bac9c4b15394b616d') }
---
shards
[
  {
    _id: 'shard1ReplSet',
    host: 'shard1ReplSet/host.docker.internal:27031',
    state: 1,
    topologyTime: Timestamp({ t: 1731961994, i: 10 }),
    replSetConfigVersion: Long('1')
  },
  {
    _id: 'shard2ReplSet',
    host: 'shard2ReplSet/host.docker.internal:27032',
    state: 1,
    topologyTime: Timestamp({ t: 1731962001, i: 9 }),
    replSetConfigVersion: Long('-1')
  }
]
---
active mongoses
[ { '8.0.3': 1 } ]
---
autosplit
{ 'Currently enabled': 'yes' }
---
balancer
{
  'Currently enabled': 'yes',
  'Currently running': 'no',
  'Failed balancer rounds in last 5 attempts': 0,
  'Migration Results for the last 24 hours': 'No recent migrations'
}
---
shardedDataDistribution
[
  {
    ns: 'testDB.users',
    shards: [
      {
        shardName: 'shard2ReplSet',
        numOrphanedDocs: 0,
        numOwnedDocuments: 515,
        ownedSizeBytes: 24720,
        orphanedSizeBytes: 0
      },
      {
        shardName: 'shard1ReplSet',
        numOrphanedDocs: 0,
        numOwnedDocuments: 485,
        ownedSizeBytes: 23280,
        orphanedSizeBytes: 0
      }
    ]
  },
  {
    ns: 'config.system.sessions',
    shards: [
      {
        shardName: 'shard1ReplSet',
        numOrphanedDocs: 0,
        numOwnedDocuments: 10,
        ownedSizeBytes: 990,
        orphanedSizeBytes: 0
      }
    ]
  }
]
---
databases
[
  {
    database: { _id: 'config', primary: 'config', partitioned: true },
    collections: {
      'config.system.sessions': {
        shardKey: { _id: 1 },
        unique: false,
        balancing: true,
        chunkMetadata: [ { shard: 'shard1ReplSet', nChunks: 1 } ],
        chunks: [
          { min: { _id: MinKey() }, max: { _id: MaxKey() }, 'on shard': 'shard1ReplSet', 'last modified': Timestamp({ t: 1, i: 0 }) }
        ],
        tags: []
      }
    }
  },
  {
    database: {
      _id: 'testDB',
      primary: 'shard2ReplSet',
      version: {
        uuid: UUID('bcc98b8c-0e5b-4b83-992a-701a910d0dc7'),
        timestamp: Timestamp({ t: 1731962464, i: 2 }),
        lastMod: 1
      }
    },
    collections: {
      'testDB.users': {
        shardKey: { name: 'hashed' },
        unique: false,
        balancing: true,
        chunkMetadata: [
          { shard: 'shard1ReplSet', nChunks: 1 },
          { shard: 'shard2ReplSet', nChunks: 1 }
        ],
        chunks: [
          { min: { name: MinKey() }, max: { name: Long('0') }, 'on shard': 'shard2ReplSet', 'last modified': Timestamp({ t: 1, i: 0 }) },
          { min: { name: Long('0') }, max: { name: MaxKey() }, 'on shard': 'shard1ReplSet', 'last modified': Timestamp({ t: 1, i: 1 }) }
        ],
        tags: []
      }
    }
  }
]
```
- The rejoined shard (`shard1ReplSet`) will seamlessly integrate back into the cluster.
- Queries that previously failed due to the shard being down will execute successfully.
- The balancer, if necessary, will ensure that the shards have an even distribution of data over time.


