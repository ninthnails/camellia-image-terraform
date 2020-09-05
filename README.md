# Camellia Image Terraform module

Terraform module which creates Amazon Machine Images (AMI) for Camellia using HashiCorp Packer and AWS CodeBuild.

## Available Features
* Apache Kafka version 2.4.1
* Based on Amazon Linux 2 with performance tuning 
* Multi Availability Zone support
* Support EBS, Instance Storage and root volume as storage types  
* Support `gp2`, `io1`, `st1` and `ephemeral` storage volume types
* XFS filesystem tune for better performance
* Swap file setup 
* Support multiple type and number of volumes
* Service integrated with systemd init system
* Integrate [collectd](https://collectd.org/) for OS metrics, support Graphite and Prometheus publishing
* Integrate Amazon CloudWatch Agent for publishing logs

## Usage

```hcl
module "camellia-image" {
  source = "ninthnails/camellia-image/aws"
  version = "1.1.0"
  prefix = "camellia"
  packer_template = "aws-private.json"
  packer_instance_type = "t3a.micro"
  vpc_id = "vpc-54e70a3249a84d75f"
  subnet_ids = ["subnet-eb537de7850039a7f"]
  tags = {
    Terraform = "true"
  }
}
```

CodeBuild run is not automatically triggered. You need to execute the build command the command output.
For example:

```shell script
aws --region us-east-2 codebuild start-build --project-name camellia-kafka-automation-packer --source-version xyz...
```

or even shorter:
```shell script
$(terraform output packer_build_command)
```
