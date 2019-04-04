
# How to

    docker-compose up -d
    docker exec -it clickhouse-replication-example_ch-client_1 clickhouse-client -h ch-server-1


# explanation

Clickhouse has a weird replication setup.

In this example `r0` and `r1` represent local databases on the clickhouse
server.  In the environment, we're telling clickhouse the following:

    CH_SHARD_R0: Primary shard id assigned to database r0
    CH_REPLICA_R0: Replica number for shard above (primary for 00)
    CH_SHARD_R1: Primary shard id assigned to database r1
    CH_REPLICA_R1: Replica number for shard above (secondary for 01)

For every shard stanza in config.xml, the pair of replicas must contain the
same data.  In this example, this is the complete configuration in laymans:

  ch-server-1 database r0 holds shard 00 replica 00
  ch-server-3 database r1 holds shard 00 replica 01

  ch-server-2 database r0 holds shard 01 replica 00
  ch-server-1 database r1 holds shard 01 replica 01

  ch-server-3 database r0 holds shard 02 replica 00
  ch-server-2 database r1 holds shard 02 replica 01

Corresponding with 3 shards, replicated 2 ways.
