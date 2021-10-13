# cloudformation-vpc-pub-and-priv

Simple AWS CloudFormation script that will create vpc with private and public subnet, and a NAT Gateway

## Diagram

![AWS CloudFormation - Diagram](https://github.com/johnnynguyen541/cloudformation-vpc-pub-and-priv/blob/main/docs/infrastructure-diagram.png?raw=true)

## Description

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

## Project Files

`docs/cli-example.txt` - CLI example

`docs/infrastructure-diagram.png` - Project Outline

`img` - Project Images

`create.sh` - Create AWS CloudFormation Stack

`LICENSE` - License

`README.md` - README file with instructions

`update.sh` - Update AWS CloudFormation Stack

`vpc-pub-priv-parameters.json` - Parameters File

`vpc-pub-priv.yml` - AWS CloudFormation IaC Code

## Requirements

* AWS Account Required

* AWS CLI installed

* AWS Default VPC with subnet created in account

## How to use

See `cli-example.txt` for example run.

To Create Stack:

`./create.sh [STACK_NAME] vpc-pub-priv.yml vpc-pub-priv-parameters.json`

Check Status:

`aws cloudformation describe-stacks --stack-name [STACK_NAME]`

To Update Stack:

`./update.sh [STACK_NAME] vpc-pub-priv.yml vpc-pub-priv-parameters.json`

Delete Stack:

`aws cloudformation delete-stack --stack-name [STACK_NAME]`
