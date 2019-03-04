# Overview for Current State of OCP on AWS Workshop

The content uploaded at this repo is to allow a user to deploy the existing OCP on AWS (running on 3.9) as it exists today.

There are two sets of Ansible scripts and .pem files that allow a user to launch this workshop in the APAC region (Tokyo) or in the NA region (Oregon)

## Launch Instructions
1. Pull down the content from the git repo that includes all of the ansible scripts and .pem files required to launch the environment:
```
git clone -b AWS-OCP-LAB https://github.com/mann1mal/ocp-on-aws-content
```

2. Edit your the oregon.yml or tokyo.yml file so the workshop can be launched in the correct region with the correct number of "students" (max 100). The following variables are required to launch to playbook as is:

```
ec2_access_key:
ec2_secret_key:
student_count: ##this is the number of instances you'd like to deploy for the workshop
student_start_count: ##this is the number which the student count starts (if 1, student-1 is first)
```

3. Source the apac or na ssh key, depending on where the workshop is launched:
```
eval `ssh-agent` && ssh-add <location>.pem && ssh-add -l
```
4. Run the Ansible script with the variable file:
```
ansible-playbook -vvv -e @<location>.yml aws_lab_launch.yml
```
