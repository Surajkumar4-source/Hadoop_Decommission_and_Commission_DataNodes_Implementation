
# Decommissioning a Node in Hadoop

Decommissioning is the process of gracefully removing a DataNode from a Hadoop cluster, ensuring data integrity by replicating its stored data to other nodes before shutting it down. This feature helps maintain high availability and fault tolerance, particularly during hardware maintenance or scaling down the cluster.

## Why Decommission a Node?

- **Upgrades without risking data loss.**
- **Cluster Scaling: Remove nodes when downsizing or reorganizing cluster resources.**
- **Fault Management: Decommission malfunctioning nodes to protect data and prevent system interruptions.**
- **Decommissioning allows data to be replicated to other nodes before the DataNode is stopped, ensuring continuity of data availability and reducing the risk of data loss.**

## Steps to Decommission a Node:

### 1. Update the Exclude File
Add the hostname or IP of the DataNode to the `dfs.exclude` file, typically configured in `hdfs-site.xml`.

### 2. Refresh the NameNode
Run the following command on the NameNode to recognize the excluded node:
```bash
hdfs dfsadmin -refreshNodes
```

### 3. Monitor Decommissioning Progress
The DataNode will stop accepting new data, and existing data will be replicated to other DataNodes. Monitor this process on the Hadoop Web UI (Decommissioning Nodes section).

### 4. Stop the DataNode
Once decommissioning is complete, stop the DataNode service on the node:
```bash
hadoop-daemon.sh stop datanode
```

### 5. Remove the Node from Configuration (Optional)
After decommissioning, remove the node from `dfs.exclude` if it's no longer needed.

Using these steps helps maintain Hadoop cluster stability and ensures data availability through replication during node removal and addition.

---


&nbsp;
&nbsp;

# Implementation Steps to Decommission a DataNode from Hadoop Cluster

1. **Identify the DataNode**:
   Determine the hostname or IP address of the DataNode you want to delete. You can check the DataNode status using:
   ```bash
   hdfs dfsadmin -report
   ```

2. **Safely Decommission the DataNode**:
   To safely remove a DataNode, add it to the decommission list in the `hdfs-site.xml` configuration file on the NameNode.
   ```bash
   nano /etc/hadoop/hdfs-site.xml
   ```
   Add the following configuration (if it doesn't already exist):
   ```xml
   <property>
       <name>dfs.hosts.exclude</name>
       <value>/etc/hadoop/dfs.hosts.exclude</value>
   </property>
   ```

3. **Create the Exclude File (on the manager node)**:
   Create or edit the exclude file (e.g., `/etc/hadoop/dfs.hosts.exclude`):
   ```bash
   nano /etc/hadoop/dfs.hosts.exclude
   ```
   Add the hostname or IP address of the DataNode you want to delete (e.g., `node2`).

4. **Restart the NameNode**:
   After updating the configuration, restart the NameNode to apply the changes:
   ```bash
   hadoop-daemon.sh restart namenode    or   hdfs --daemon start datanode
   ```

5. **Verify Decommissioning**:
   Check the status of the DataNodes to ensure the DataNode is decommissioned:
   ```bash
   hdfs dfsadmin -report
   ```

6. **Stop the DataNode**:
   On the DataNode you want to remove, stop the DataNode service:
   ```bash
   hadoop-daemon.sh stop datanode
   ```

7. **Remove DataNode from Cluster**:
   Optionally, you can uninstall or remove the Hadoop directory configuration from the DataNode server if it will no longer be part of the cluster.




&nbsp;
&nbsp;


## If you want to agin commision that DataNode -- Follow the below steps:   

1. **First just remove the hostname or IP address of the DataNode from this file**
```
nano /etc/hadoop/dfs.hosts.exclude
```

2. **Restart the NameNode**:
   After removing , restart the NameNode to apply the changes:
   ```bash
   hadoop-daemon.sh restart namenode    or   hdfs --daemon start datanode
   ```

3. **Verify Commissioning**:
   Check the status of the DataNodes to ensure the DataNode is decommissioned:
   ```bash
   hdfs dfsadmin -report
   ```








<br>
<br>
<br>
<br>



**👨‍💻 𝓒𝓻𝓪𝓯𝓽𝓮𝓭 𝓫𝔂**: [Suraj Kumar Choudhary](https://github.com/Surajkumar4-source) | 📩 **𝓕𝓮𝓮𝓵 𝓯𝓻𝓮𝓮 𝓽𝓸 𝓓𝓜 𝓯𝓸𝓻 𝓪𝓷𝔂 𝓱𝓮𝓵𝓹**: [csuraj982@gmail.com](mailto:csuraj982@gmail.com)





<br>
