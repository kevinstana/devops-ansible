This README does not describe how to make a Jenkins pipeline.
# Hosts
The `ansible_hosts` in `hosts.yaml` should be the same as the hosts in the `.ssh/config` file of the control node.

## Vagrant Option
You can use vagrant to create VMs on your machine. To do so, cd into the `vagrant` directory and run the following command:
```bash
vagrant up
```
Once the VMs have been created, use the following command to be able to access them using SSH:
```bash
vagrant ssh-config >> .ssh/config
```
### CAUTION
Be extremely careful with the previous command. It must be exactly '>>'.
If there is only '>' the contents of your .ssh/config will be deleted and you will lose valuable information!
Using '>>' appends to the .ssh/config file.

# Deployment with just Ansible
Supposing you are in the `playbooks` directory, to deploy using just Ansible, run the following commands:

## Postgres
```bash
ansible-playbook -l db postgres.yaml
```

## Spring Application
```bash
ansible-playbook -l backend -e db_url=<DATABASE_IP>:<DATABASE_PORT> spring.yaml
```
Make sure to replace <DATABASE_IP> with the IP address of the machine that the postgres database runs on.
Also, replace <DATABASE_PORT> with the port that the postgres database listens to, usually this is port 5432.

## Angular Application
```bash
ansible-playbook -l frontend -e backend_server_url=<BACKEND_IP>:<BACKEND_PORT> angular.yaml
```
Make sure to replace <BACKEND_IP> with the IP address of the machine that the Spring Application runs on.
Also, replace <BACKEND_PORT> with the port that the Spring Application listens to. In the applciation I use, this is port 9090.

Now open a browser and navigate to <BACKEND_URL>:<BACKEND_PORT>. You should see a login page.

# Deployment with Ansible - Docker
Supposing you are in the `playbooks` directory, to deploy using Ansible and Docker, run the following commands:

## Docker-compose
```bash
ansible-playbook -l docker-vm spring-angular-docker.yaml
```
This playbook will first make sure Docker is installed on the target machine, and then proceed by executing the docker-compose.yaml file.

Now open a browser and navigate to the IP address of the machine that the containers where deployd to. For the port, use port 9000.
So for example, navigate to my-super-app:9000. You should see a login page.
