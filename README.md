# artirix-devops-technical-test
This is to answer the test posed to me to set up infrastructure as code. I have decided to try to do this in Terraform on AWS as I know AWS, however Terraform is a new challange for me.

## How it stands
* Three nodes are spun up on AWS and installs ElasticSearch, all on defaults at the moment
* ELB is set up to go to 9200
* Security groups to ELB and systems are set up so that 22 and 9200 are only open

## Things to Do
* Get ElasticSearch fully configured as a cluster
* Optimise the Terraform config so more procedural
* Include Packer as this is a recommended tool to use with Terraform
* Get the Contract

## How to run it.
Since this is based on the AWS-Two-Tier example, it can be run with the following command:

terraform apply -var 'key_name=terraform' -var 'public_key_path=~/.ssh/id_rsa.pub'

AWS credentials are pulled from the AWS cli config.


## Why AWS Elasticsearch?
* It automatically replaces failed nodes: You don’t need to get paged in the middle of the night, spin a new node and add it to the cluster
* You can add/remove nodes through API/Console/Terraform: Otherwise, you’ll have to make sure you have all the automation in place so that when you spin a node you don’t spend extra time manually installing and configuring Elasticsearch
* You can manage access rights via IAM: This is easier than setting up a reverse proxy or a security addon (Cost Implication for ELB usage too)
* Daily snapshots to S3 are included: This saves you the time and money to set it up (and the storage cost) for what is a mandatory step in most use-cases
* CloudWatch monitoring included
* No need for LoadBalancer as this is managed by the cluster

## Why to Install Your Own Elasticsearch?
* On demand equivalent instances are cheaper by ~29%: The delta differs from instance to instance
* More instance types and sizes are available: You can use bigger i2 instances than AWS Elasticsearch, and you have access to the latest generation of c4 and m4 instances. This way, you are likely to scale further, especially with logs and metrics (more specific hardware recommendations and Elasticsearch tuning here)
* You can change more index settings beyond analysis and number of shards/replicas. For example, delayed allocation, which is useful when you have a lot of data per node. You can also change the settings of all indices at once by hitting the /_settings endpoint. By properly utilizing various settings Elasticsearch makes available you can better optimize your setup for your particular use case, make better use of underlying resources, and thus drive the cost down further.
* You can change more cluster-wide settings, such as number of shards to rebalance at once
* You can use a more comprehensive Elasticsearch monitoring solution. Currently, CloudWatch only collects a few metrics, such as cluster status, number of nodes and documents, heap pressure and disk space. For most use-cases, you’ll need more info, such as the query latency and indexing throughput. And when something goes wrong, you’ll need more insight on JVM pool sizes, cache sizes, Garbage Collection or you may need to profile Elasticsearch
* You can have clusters of more than 20 nodes.

## Conclusion
If you are looking to set up a small Elasticsearch cluster quickly (Less than 20 nodes) or starting out with Elasticsearch, then the AWS service is the way to go. 

However once the infrastructure code has been written and a possible image is set up with AWS this can allow for almost unlimited scaling.

Both these solutions have cost implications and will need to be considered when setting up an Elasticsearch Cluster. Time setting up infrastructure (EC2 deployment) vs easy deployment (AWS ElasticCloud Service) plus ongoing costs.
