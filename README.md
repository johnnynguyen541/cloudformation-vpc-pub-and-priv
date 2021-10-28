# cloudformation-vpc-pub-and-priv

AWS CloudFormation scripts that creates Network and Servers:

* Network: VPC with private and public subnet, and a NAT Gateway
* Servers: EC2 instances on private subnet with Autoscaling Group, with HTTP access via Load Balancer and SSH access via Jump Box

## Diagram

![AWS CloudFormation - Diagram](https://github.com/johnnynguyen541/cloudformation-vpc-pub-and-priv/blob/main/docs/infrastructure-diagram.png?raw=true)

## Description

* Network AWS CF Script
    * Creates VPC with Input CIDR Block
    * Creates and attaches an Internet Gateway to VPC
    * Creates 2 Private and Public Subnets with Input CIDR Blocks
        * CIDR blocks in subnets must be /24 in size
        * MapPublicIpOnLaunch set to true on Public
        * MapPublicIpOnLaunch set to false on Private
    * Creates 2 NAT Gateways with Elastic IP on Public
    * Creates Routing Tables for Public/Private
        * Public sends 0.0.0.0/0 to Internet Gateway
        * Private sends 0.0.0.0/0 to NAT Gateways
* Servers AWS CF Script
    * Create EC2 Auto Scaling group in private subnet (t3.micro)
    * Security group for private EC2 (inbound port 80, 443, and 22)
    * IAM Role to allow EC2 Session Manager to access our server
    * Load Balancer in Public Network to route to Auto Scaling Groups
    * Jumpbox to SSH.

## Project Files

`docs/cli-example.txt` - CLI example

`docs/infrastructure-diagram.png` - Project Outline

`aws-cf-network-parameters.json` - Network Parameters File

`aws-cf-network.yml` - AWS CloudFormation IaC Code - Network

`aws-cf-servers-parameters.json` - Servers Parameters File

`aws-cf-servers.yml` - AWS CloudFormation IaC Code - Servers

`create.sh` - Create AWS CloudFormation Stack

`LICENSE` - License

`README.md` - README file with instructions

`update.sh` - Update AWS CloudFormation Stack


## Requirements

* AWS Account Required

* AWS CLI installed

* AWS Default VPC with Subnet Created in Account

## How to use

See `cli-example.txt` for example run.

To Create Network Stack:

`./create.sh [STACK_NAME] aws-cf-network.yml aws-cf-network-parameters.json us-west-2`

Check Status:

`aws cloudformation describe-stacks --stack-name [STACK_NAME]`

To Update Stack:

`./update.sh [STACK_NAME] aws-cf-network.yml aws-cf-network-parameters.json us-west-2`

Delete Stack:

`aws cloudformation delete-stack --stack-name [STACK_NAME]`

To Create Server Stack:

`./create.sh [STACK_NAME] aws-cf-servers.yml aws-cf-servers-parameters.json us-west-2`
