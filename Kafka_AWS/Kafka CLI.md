# Kafka CLI Kraft
### how to invoke
1. if you setup the $PATH variable correctly, then you should be able to invoke the CLI from anywhere on the computer
   ```
   ~> kafka-topics.sh
   ```  
2. Use the ``--bootstrap-server` option everywhere, instead of ``--zookeeper``
   ```
   kafka-topics --bootstrap-server localhost:9092
   ```
  ` --bootstrap-server` 是 Kafka 客户端  
  告诉 Kafka broker 的地址（host:port），去连接它，Kafka 会告诉集群的其他信息   
> 以前（Zookeeper 模式) :
> ```
> kafka-topics.sh --create \
> --topic my-topic \
> --zookeeper localhost:2181 \
> --partitions 1 --replication-factor 1
> ```
> 现在（Kraft）
> ```
> kafka-topics.sh --create \
> --topic my-topic \
> --bootstrap-server localhost:9092 \
> --partitions 1 --replication-factor 1
> ```

### Kafka Topic Management  

1. Create Kafka Topics
2. List Kafka Topics
3. Describe Kafka Topics
4. Increase Partitions in a Kafka Topic
5. Delete a Kafka Topic


