# Instructions for Deploying the OCP on AWS 3.x Workshop in your own EC2 account.

## Configure your VPC

Steps for configuring a VPC to support this workshop can be found in the "build-ami" doc above.

The first 12 minutes of the [following bluejeans link](https://redhat.bluejeans.com/playback/s/PRVnPh7BNsK6SjQHFoQjLShFqWev9ALivpMQSzZa6z7EIfLkL9FALKyMIJgHINuf) walks through the creation of the VPC for the workshop environment.

## Configure a domain via Route53 to be used in the workshop

You will also need to allocate a domain in Route53 to be used when provisioning the workshop so the instances can be assigned human readible hostnames.

## Build your base AMI to be used to deploy the workshop instances

Directions for this are also available in the "build-ami" doc above

## Create an IAM role to attach service broker to OCP environment

Create an IAM user with the "aws-servicebroker-cfn-deploy-role" to be used assigned to the EC2 instances during the provision process.

## Instructions for launching the workshop

1. Pull down the content from the git repo that includes all of the ansible scripts and .pem files required to launch the environment:
```
$ git clone https://github.com/mann1mal/ocp-on-aws-content
```
2. Navigate into the `ansible-scripts` directory
```
$ cd ~/ocp-on-aws-content/ansible-scripts
```
3. If running this lab in the AWS-owned account:

Edit your the `oregon.yml` or `tokyo.yml` file so the workshop can be launched in the correct region with the correct number of "students" (max 100). The following variables are required to launch to playbook:

```
ec2_access_key:
ec2_secret_key:
student_count: ##this is the number of instances you'd like to deploy for the workshop
student_start_count: ##this is the number which the student count starts (if 1, student-1 is first)

# AWS location information
ami_inst_type: t2.xlarge
aws_vpc_name: "<VPC Name>"
aws_route_table: "<route table name>"
aws_subnet_id: "<subnet-id>"
aws_region: "<aws-region-name>" 
aws_sec_group: "<gateway-name>"
aws_vpc_cidr_block: "<VPC-CIDR-block>" 
aws_subnet_cidr: "<subnet-CIDR-block>"
aws_subnet_name: "<subnet-name>"
aws_key_name: "<key-name>"
aws_az_1: "<EC2-availability-zone>"
instance_profile_name: <role-name-to-attach-service-broker> #this is the role name configured above that allows us to use the service broker to attach AWS services to the OCP environment
```
4. Source the `oregon.pem` or `tokyo.pem` ssh key, depending on where the workshop is launched. The ansible scripts that launch the lab need to be able to ssh into the instances to configure services:
```
$ eval `ssh-agent` && ssh-add <location>.pem && ssh-add -l
```
5. Run the Ansible script with the location-specific variable file:
```
$ ansible-playbook -vvv -e @<location>.yml aws_lab_launch.yml
```
