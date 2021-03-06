# Overview for Current State of OCP on AWS Workshop

The content uploaded at this repo is to allow a user to deploy the OCP on AWS workshop infrastructure (running on 3.9) as it exists today in the AWS-owned EC2 account. If you'd like to set this up in your own EC2 account, [click here](https://github.com/mann1mal/ocp-on-aws-content/tree/master/AMI-Build) for additional instructions.

There are two sets of Ansible scripts and .pem files that allow a user to launch this workshop in the APAC region (Tokyo) or in the NA region (Oregon)

## Launch Instructions
1. Pull down the content from the git repo that includes all of the ansible scripts and .pem files required to launch the environment:
```
$ git clone https://github.com/mann1mal/ocp-on-aws-content
```
2. Navigate into the `ansible-scripts` directory
```
$ cd ~/ocp-on-aws-content/ansible-scripts
```
3. Edit your the `oregon.yml` or `tokyo.yml` file so the workshop can be launched in the correct region with the correct number of "students" (max 100). The following variables are required to launch to playbook as is (all the rest of the default variables can remaind unchanged):

```
ec2_access_key:
ec2_secret_key:
student_count: ##this is the number of instances you'd like to deploy for the workshop
student_start_count: ##this is the number which the student count starts (if 1, student-1 is first)
```
4. Set the correct permissions on the .pem files:
```
$ chmod 0600 *.pem
```
5. Source the `oregon.pem` or `tokyo.pem` ssh key, depending on where the workshop is launched. The ansible scripts that launch the lab need to be able to ssh into the instances to configure services:
```
$ eval `ssh-agent` && ssh-add <location>.pem && ssh-add -l
```
6. Run the Ansible script with the location-specific variable file:
```
$ ansible-playbook -vvv -e @<location>.yml aws_lab_launch.yml
```
