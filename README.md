Zip maven sprong boot project 
Example URL to send message
http://localhost:8080/send?message=orange
Group id name - testJava

Steps: Go in bin/windows folder

1 Start Zookeeper 
zookeeper-server-start.bat ..\..\config\zookeeper.properties

2 Start Broker
kafka-server-start.bat ..\..\config\server.properties

3 Create topic
kafka-topics.bat --create --topic yourTopicName --bootstrap-server localhost:9092 --replication-factor 1 --partitions 3

replication-factor depends upon number of brokers
if replication will be more than 1 then there will be leader for each partition and will be syncronized by ISR (in sync replica)

partitions choose as per requirement data will be inserted in round robin method

commands:
connect producer- kafka-console-producer.bat --broker-list localhost:9092 --topic myTopicname
connect consumer- kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic myTopicname --from-beginning

if key value pair
kafka-console-producer.bat --broker-list localhost:9092 --topic myTopicname --property "key.separator=-" --property "parse.key=true"
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic myTopicname --from-beginning -property "key.separator=-" --property "print.key=false"

list all topics ----->  kafka-topics.bat --bootstrap-server=localhost:9092 --list
list all consumer groups -----> kafka-consumer-groups.bat --bootstrap-server localhost:9092 --list
describe a topic ----->  .\kafka-topics.bat --describe --topic myTopicname --bootstrap-server localhost:9092    
start producer with key ----->   kafka-console-producer.bat --broker-list localhost:9092 --topic myTopicname --property "key.separator=-" --property "parse.key=true"
start consumer with group name ----->  kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic fruits --group testJava


For multiple brokers:

create multipe server.properties
example: server.properties, server1.properties server2.properties

Edit the port number by editing property listeners 
example listeners=PLAINTEXT://:9093

change log.dirs path for each server

Run all servers:
.\bin\windows\kafka-server-start.bat .\config\server.properties
.\bin\windows\kafka-server-start.bat .\config\server1.properties
.\bin\windows\kafka-server-start.bat .\config\server2.properties


create topic on multiple servers:
.\bin\windows\kafka-topics.bat --create --bootstrap-server localhost:9092,localhost:9093,localhost:9094 --replication-factor 3 --partitions 3 --topic your_topic_name

replication-factor 3 for 3 servers 
each topic has 3 partitions 
