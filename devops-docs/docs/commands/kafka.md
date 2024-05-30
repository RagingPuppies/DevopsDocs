# Kafka Cheat Sheet

## Configurations

Get a list of all topics and apply the same configuration to all (e.g., remove throttling):

    /opt/kafka/kafka/bin/kafka-topics.sh --list --bootstrap-server localhost:9092 | xargs -I % /opt/kafka/kafka/bin/kafka-configs.sh --bootstrap-server localhost:9092 --alter --delete-config follower.replication.throttled.replicas,leader.replication.throttled.replicas --topic %

Set retention:

    bin/kafka-configs.sh --bootstrap-server localhost:9092 --topic topic_a --add-config retention.ms=60000 --alter

Set IO threads:

    bin/kafka-configs.sh --bootstrap-server localhost:9092 --entity-type brokers --entity-default --alter --add-config num.io.threads=96

Get configurations for broker:

    bin/kafka-configs.sh --describe --bootstrap-server localhost:9092 --entity-type brokers --entity-default

## Force Leader Election in Disaster

If you've ever been in a situation where a few brokers died, and there's nothing to do but force Kafka to move a follower partition into a leader state even though it is not fully in sync (**data loss**):

Elect a specific partition:

    bin/kafka-leader-election.sh --bootstrap-server localhost:9092 --topic topic_a --partition 22 --election-type UNCLEAN

Elect all non-synced partitions:

    bin/kafka-leader-election.sh --bootstrap-server localhost:9092 --all-topic-partitions --election-type UNCLEAN

Revert the election type:

    bin/kafka-leader-election.sh --bootstrap-server localhost:9092 --all-topic-partitions --election-type PREFERRED
