Neo4j HA Cluster Deployment Using Ansible
=====================

Ansible playbook for Neo4j HA Cluster on AWS/Eucalyptus.  The AMI/EMI used should be an Ubuntu Cloud Image.  This was tested using Ubuntu 12.04 Precise.  This playbook deploys the following:

* a Neo4j HA cluster
* an HA proxy server that serves as a single point to access the Neo4j HA cluster

### Pre-reqs

Populate vars/ec2-config with either Eucalyptus/AWS information.  vars/ec2-config contains the following variables:

```
keypair: <EC2/Eucalyptus Keypair>
ec2_access_key: <EC2_ACCESS_KEY>
ec2_secret_key: <EC2_SECRET_KEY>
ec2_url: <EC2_URL>
instance_type: m1.small
security_group: <AWS/Eucalyptus Security Group>
image: <AMI/EMI>
```

Any config changes for HA Proxy or the Neo4j cluster can be found under the vars directory.  

### Usage

After cloning the repository, and populating the config files under vars directory, run the following command:

```
 ansible-playbook ansible-neo4j-cluster/neo4j-cluster.yml
 --private-key=<AWS/Eucalyptus Private Key file> --extra-vars "node_count=3"
```

To change the number of nodes for the cluster, change node_code to equal that number (e.g. node_code=7).  

After the playbook finishes, there will be an URL provided to access the cluster - similar to the example below:

```
TASK: [Display HAProxy URL] *********************
changed: [23.22.248.75] => {"changed": true, "cmd": 
"echo \"HAProxy URL for Neo4j - http://ec2-23-22-248-75.compute-1.amazonaws.com/webadmin/#/info/org.neo4j/High%20Availability/\" ", "delta": "0:00:00.006835", "end": "2013-03-30 19:54:31.104320", 
"rc": 0, "start": "2013-03-30 19:54:31.097485", "stderr": "", 
"stdout": "HAProxy URL for Neo4j - http://ec2-23-22-248-75.compute-1.amazonaws.com/webadmin/#/info/org.neo4j/High%20Availability/"}
```

To get the status of the Neo4j cluster, run the following command:

```
curl -H "Content-Type:application/json" -d '["org.neo4j:*"]' http://ec2-23-22-248-75.compute-1.amazonaws.com/db/manage/server/jmx/query
```

For more information, please refer to the documentation for configuring/managing the Neo4j cluster - http://docs.neo4j.org/chunked/milestone/operations.html
