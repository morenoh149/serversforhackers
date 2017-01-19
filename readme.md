# Servers for Hackers
Some notes while working through https://serversforhackers.com

## Ansible
Instead of using the default location for the hosts file(`/etc/ansible/hosts`) we use
the current directory

`$ ansible web -i digital_ocean_hosts`

### ssh keys
When the digital ocean droplets were created an ssh key was generated and pasted into
them. It resides in `./digital_ocean_rsa{.pub}`.
To log into the droplets run

`$ ssh root@104.236.98.173 -i ./digital_ocean_rsa`

#### Example commands
* shell
  * `$ ansible all -m shell -a "ping -c 3 localhost" -u root -i digital_ocean_hosts --private-key=digital_ocean_rsa`
* apt install nginx
  * `$ ansible all -m apt -a "pkg-nginx state=latest update_cache=true" -u root --private-key=digital_ocean_rsa`
* shell aws
  * `$ ansible web -m shell -a "ping -c 3 localhost" -u ubuntu -i aws_hosts --private-key=./aws/capsule_workstation.pem`
  * requires having ssh'd into each inventory vm at least once previously
* apt install nginx on aws
  * `$ ansible web -m apt -a "pkg=nginx state=latest update_cache=true" -u ubuntu --private-key=aws/capsule_workstation.pem -i aws_hosts --become`

### OS
We use ubuntu 14 because ansible doesn't currently execute via python3. The default
python was also upgraded in ubuntu 16.

### Ubuntu on AWS
To connect do
`$ ssh -i aws/capsule_workstation.pem root@ec2-35-167-14-104.us-west-2.compute.amazonaws.com`

Or
`$ ssh -i aws/capsule_workstation.pem ubuntu@52.26.54.229`

The pem file is stored in the aws directory.

Ansible requires the public IP of the vm in the inventory file.

### Playbooks
Sample command:
* Installs Nginx on droplets

`$ ansible-playbook --private-key=digital_ocean_rsa nginx.yml -i digital_ocean_hosts`
* Installs Nginx on aws ec2

`$ ansible-playbook --private-key=aws/capsule_workstation.pem aws_nginx.yml -i aws_hosts`
