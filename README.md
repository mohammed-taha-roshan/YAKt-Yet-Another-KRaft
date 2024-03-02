**YAKt-Yet-Another-KRaft**

Libraries used:
1. Pyraft - leader election, raft node structure, log replication
2. Telnetlib - used get, set methods for log replication
   
Metadata Records used:
1. RegisterBrokerRecord:
2. Topic Record
3. Partition Record
4. ProducerID Record
5. Change Broker Record

End points created:
1. /register_broker - Responsible for the registration of brokers. Added additional timestamp for snapshot purposes.
2. /get_active_brokers - Responsible for the retrieving brokers with status 'ALIVE'
3. /create_topic - Responsible for creating topic
4. /get_topic_by_name/<string:topic_name> - Responsible for retrieving the topic according to the name specified by the client.
5. /create_partition - Responsible for creating partition
6. /register_producer - Responsible for the registration of producers
7. /update_broker - Responsible for updating the current node state and appending to 'change broker record' with an increment to epoch.
8. /unregister_broker/<string:broker_id> - Responsible for changing the broker status to 'CLOSED' and delete the broker from the node. Note: Broker information can still be retrieved from metadata if required.
9. /broker_mgmt/<float:previous_offset> - Responsible for fetching two different snapshots:
    i. Current State Snapshot - If timestamp offset is less than 10 minutes, retrieves the data present in the nodes
   ii. Complete State Snapshot - If timestamp offset is greater than 10 minutes, retrieves the state information from the metadata.
10. /client_mgmt/<float:previous_offset> - Responsible for fetching two different snapshots:
    i. Current State Snapshot - If timestamp offset is less than 10 minutes, retrieves the data present in the nodes
   ii. Complete State Snapshot - If timestamp offset is greater than 10 minutes, retrieves strictly the topics, partitions, broker information. 
    

Steps for initial node setup:
python3 my_raft_node.py -i 1 -a 127.0.0.1:5010  

python3 my_raft_node.py -i 2 -a 127.0.0.1:5020 -e 1/127.0.0.1:5010

python3 my_raft_node.py -i 3 -a 127.0.0.1:5030 -e 1/127.0.0.1:5010,2/127.0.0.1:5020 

python3 my_raft_node.py -i 4 -a 127.0.0.1:5040 -e 1/127.0.0.1:5010,2/127.0.0.1:5020,3/127.0.0.1:5030

python3 my_raft_node.py -i 5 -a 127.0.0.1:5050 -e 1/127.0.0.1:5010,2/127.0.0.1:5020,3/127.0.0.1:5030,4/127.0.0.1:5040

To visualize leader election - kill leader node
