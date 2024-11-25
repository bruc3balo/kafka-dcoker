# KAFKA SET UP WITH DOCKER

Ensure container has permission to volumes 
```bash
sudo chmod -R 777 .data
```

# Environment Variables
## GENERAL CONFIGURATION
    
    KAFKA_KRAFT_MODE -> e.g. "true". This enables KRaft mode in Kafka.
    CLUSTER_ID -> e.g. "clusterIdHere". Unique ID for the Kafka cluster, required in KRaft mode.
    KAFKA_LOG_DIRS -> e.g. /var/lib/kafka/data. Directory where Kafka stores log data (topics, partitions, and offsets).
    KAFKA_ZOOKEEPER_CONNECT -> e.g. zookeeper:2181. Address of the ZooKeeper ensemble (only required for ZooKeeper-based setups).
    KAFKA_PROCESS_ROLES -> e.g. controller,broker. Kafka acts as both broker and controller.
    
    
## NETWORKING & LISTENERS

    KAFKA_LISTENERS -> e.g. PLAINTEXT://0.0.0.0:9092,CONTROLLER://0.0.0.0:9093. Specifies where the broker listens for client and inter-broker connections.
    KAFKA_ADVERTISED_LISTENERS -> e.g. PLAINTEXT://localhost:9092. The external address clients should use to connect to this broker.
    KAFKA_LISTENER_SECURITY_PROTOCOL_MAP -> e.g. PLAINTEXT:PLAINTEXT,CONTROLLER:PLAINTEXT. Maps listener names to their security protocols (e.g., PLAINTEXT, SASL_SSL).
    KAFKA_INTER_BROKER_LISTENER_NAME -> e.g. PLAINTEXT. Listener used for inter-broker communication.
    KAFKA_CONTROLLER_LISTENER_NAMES -> e.g. CONTROLLER. Listener(s) used for controller communication in KRaft mode.

## CONTROLLER & PRODUCER

    KAFKA_NODE_ID (KRaft) -> e.g. 1 .A unique identifier for each Kafka broker in the cluster. # KAFKA_BROKER_ID on zookeeper and KAFKA_NODE_ID on KRaft
    KAFKA_BROKER_ID  (zookeeper)-> e.g. 1
    KAFKA_CONTROLLER_QUORUM_VOTERS -> e.g."1@localhost:9093". Defines the controller voters.
    
## TOPIC & PARTITION MANAGEMENT

    KAFKA_AUTO_CREATE_TOPICS_ENABLE -> e.g. "true". Automatically create topics when a producer/consumer connects.
    KAFKA_DEFAULT_REPLICATION_FACTOR -> e.g. 1. Default replication factor for newly created topics.
    KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR -> e.g. 1. Replication factor for the offsets topic (used for consumer group management).
    KAFKA_NUM_PARTITIONS -> e.g. 1. Default number of partitions for newly created topics
    KAFKA_MIN_IN_SYNC_REPLICAS -> e.g. 0. Minimum number of replicas in sync for a write to succeed.

## LOG & RETENTION SETTINGS

    KAFKA_LOG_RETENTION_HOURS -> e.g. 168. Keep logs for 7 days.
    KAFKA_LOG_RETENTION_BYTES -> e.g. 1073741824 (1GB). Maximum size of logs before deletion
    KAFKA_LOG_SEGMENT_BYTES -> e.g. 1073741824 (1GB). Size of a single log segment file
    KAFKA_LOG_CLEANER_ENABLE -> e.g. true. Enable log compaction for compacted topics
    
## CONSUMER GROUP & PRODUCER SETTINGS
    
    KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS -> e.g. 0. Time to wait before starting a new consumer group rebalance
    KAFKA_TRANSACTION_STATE_LOG_MIN_ISR -> e.g. 1. Minimum ISR for the transaction state log topic.

## SECURITY & AUTHENTICATION

    KAFKA_SASL_ENABLED_MECHANISMS -> e.g. SCRAM-SHA-256.  SASL mechanisms to enable (e.g. PLAIN, SCRAM-SHA-256, SCRAM-SHA-512)
    KAFKA_SASL_MECHANISM_INTER_BROKER_PROTOCOL -> e.g. SCRAM-SHA-256. SASL mechanism for inter-broker communication.
    KAFKA_AUTHORIZER_CLASS_NAME (zookeeper) -> e.g. kafka.security.authorizer.AclAuthorizer. Authorizer class to enable ACLs for Kafka.
    KAFKA_ALLOW_EVERYONE_IF_NO_ACL_FOUND (zookeeper) -> e.g. true. Whether to allow all operations if no ACL is found
    KAFKA_SUPER_USERS -> e.g. User:admin. List of super users who can perform all operations.
    KAFKA_OPTS -> e.g."-Djava.security.auth.login.config=/etc/kafka/jaas.conf". Set up configuartion for 

## MONITORING & METRICS

    KAFKA_JMX_PORT -> e.g. 9999. Port for exposing JMX metrics for monitoring tools
    KAFKA_METRIC_REPORTERS -> e.g. io.prometheus.kafka.KafkaJmxReporter. Specifies which metric reporters to use (e.g., JMX, Prometheus)

## DEBUGGING & PERFORMANCE

    KAFKA_DEBUG -> e.g. true. Enable debugging for Kafka (e.g., consumer, producer)
    KAFKA_HEAP_OPTS -> e.g. -Xmx1G -Xms1G .JVM heap size for Kafka brokers

## DOCKER SPECIFIC VARIABLES

    CONFLUENT_METRICS_REPORTER_BOOTSTRAP_SERVERS -> kafka:9092. Specifies Kafka bootstrap servers for Confluent metrics
    CONFLUENT_METRICS_ENABLE -> true. Enable metrics reporting for Confluent Kafka

## SSL
    KAFKA_SSL_KEYSTORE_LOCATION -> /var/lib/kafka/ssl/kafka.keystore.jks
    KAFKA_SSL_KEYSTORE_PASSWORD -> kafka-password
    KAFKA_SSL_TRUSTSTORE_LOCATION -> /var/lib/kafka/ssl/kafka.truststore.jks
    KAFKA_SSL_TRUSTSTORE_PASSWORD -> kafka-password
    KAFKA_SSL_CLIENT_AUTH -> required


```conf
KafkaServer {
    org.apache.kafka.common.security.scram.ScramLoginModule required
    username="admin"
    password="admin";
};

# KafkaController {
#     org.apache.kafka.common.security.plain.PlainLoginModule required
#     username="controller"
#     password="controller";
# };
```