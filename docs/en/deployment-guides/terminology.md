---
slug: /en/architecture/introduction
sidebar_label: Introduction
title: Introduction
sidebar_position: 1
---
import ReplicationShardingTerminology from '@site/docs/en/_snippets/_replication-sharding-terminology.md';

These deployment examples are based on the advice provided to ClickHouse users by the ClickHouse Support and Services organization.  These are working examples, and we recommend that you try them and then adjust them to suit your needs.  You may find an example here that fits your requirements exactly. Alternatively, you may have a requirement that your data is replicated three times instead of two, you should be able to add another replica by following the patterns presented here.

<ReplicationShardingTerminology />

## Examples

### Basic

- The [**Scaling out**](/docs/en/deployment-guides/horizontal-scaling.md) example shows how to shard your data across two nodes, and use a distributed table.  This results in having data on two ClickHouse nodes.  The two ClickHouse nodes also run ClickHouse Keeper providing distributed synchronization.  A third node runs ClickHouse Keeper standalone to complete the ClickHouse Keeper quorum.

### Intermediate

- Coming soon

### Advanced

- Coming soon
