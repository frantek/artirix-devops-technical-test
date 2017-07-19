# artirix-devops-technical-test
This is to answer the test posed to me to set up infrastructure as code. I have decided to try to do this in Terraform on AWS as I know AWS, however Terraform is a new challange for me.

## How it stands
* Three nodes are spun up on AWS and installs ElasticSearch, all on defaults at the moment
* ELB is set up to go to 9200
* Security groups to ELB and systems are set up so that 22 and 9200 are only open

##Things to Do
* Get ElasticSearch fully configured as a cluster
* Optimise the Terraform config so more procedural
* Include Packer as this is a recommended tool to use with Terraform
* Get the Contract

## How to run it.
Since this is based on the AWS-Two-Tier example, it can be run with the following command:

terraform apply -var 'key_name=terraform' -var 'public_key_path=~/.ssh/id_rsa.pub'

AWS credentials are pulled from the AWS cli config.