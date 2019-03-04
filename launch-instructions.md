Launch Instructions

1. Edit your the oregon.yml or tokyo.yml file so the workshop can be launched in the correct region with the correct number of "students" (max 100)

The following variables are required to launch to playbook as is:

ec2_access_key:
ec2_secret_key:

student_count: ##this is the number of instances you'd like to deploy for the workshop
student_start_count: ##this is the number which the student count starts (if 1, student-1 is first)

2. Source the apac or na ssh key, depending on where the workshop is launched:

# eval `ssh-agent` && ssh-add <location>.pem && ssh-add -l

3. Run the Ansible script with the variable file:

ansible-playbook -vvv -e @<location>.yml aws_lab_launch.yml
