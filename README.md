# cloudformation-vpc-pub-and-priv

AWS CloudFormation scripts that creates Network and Servers:

* Network: VPC with private and public subnet, and a NAT Gateway
* Servers: EC2 instance on private subnet, with HTTP access via Load Balancer and SSH access via Jump Box

## Diagram

![AWS CloudFormation - Diagram](https://github.com/johnnynguyen541/cloudformation-vpc-pub-and-priv/blob/main/docs/infrastructure-diagram.png?raw=true)

## Description

* Network AWS CF Script
    * Creates VPC with Input CIDR Block
    * Creates and attaches an Internet Gateway to VPC
    * Creates a Private and Public Subnet with Input CIDR Blocks
        * CIDR blocks in subnets must be /24 in size
        * MapPublicIpOnLaunch set to true on Public
        * MapPublicIpOnLaunch set to false on Private
    * Creates NAT Gateway with Elastic IP on Public
    * Creates 2 Routing Tables for Public/Private
        * Public sends 0.0.0.0/0 to Internet Gateway
        * Private sends 0.0.0.0/0 to NAT Gateway
* Servers AWS CF Script
    * Create EC2 Instance in private instance (t3.micro)
    * Security group for private EC2 (inbound port 80 and 22)
    * IAM Role to allow EC2 Session Manager to access our server
    * Load Balancer in Public Network to route to Private EC2 Server
    * Jumpbox to SSH.

## Project Files

`docs/cli-example.txt` - CLI example

`docs/infrastructure-diagram.png` - Project Outline

`img/` - Project Images

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

`./create.sh [STACK_NAME] aws-cf-network.yml aws-cf-network-parameters.json`

Check Status:

`aws cloudformation describe-stacks --stack-name [STACK_NAME]`

To Update Stack:

`./update.sh [STACK_NAME] aws-cf-network.yml aws-cf-network-parameters.json`

Delete Stack:

`aws cloudformation delete-stack --stack-name [STACK_NAME]`

To Create Server Stack:

`./create.sh [STACK_NAME] aws-cf-servers.yml aws-cf-servers-parameters.json`
