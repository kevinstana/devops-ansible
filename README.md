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
vagrant ssh-config >> ~/.ssh/config
```
### CAUTION
Be extremely careful with the previous command. It must be exactly `>>`.
If there is only `>` the contents of your `.ssh/config` will be deleted and you will lose valuable information!
Using `>>` appends to the `.ssh/config` file.

# Deployment with just Ansible
Supposing you are in the `devops-ansible` directory, to deploy using just Ansible, run the following commands:

## Postgres
```bash
ansible-playbook -l db playbooks/postgres.yaml
```

## Spring Application
```bash
ansible-playbook -l backend -e db_url=<DATABASE_URL> playbooks/spring.yaml
```
Make sure to replace `<DATABASE_URL>` with the IP address of the machine that the postgres database runs on.  
If you are using the VMs from the Vagrantfile, then the URL is `192.168.56.121`.  

## Angular Application
```bash
ansible-playbook -l frontend -e backend_server_url=http://<BACKEND_IP>:<BACKEND_PORT> playbooks/angular.yaml
```
Make sure to replace `<BACKEND_IP>` with the IP address of the machine that the Spring Application runs on.  
Also, replace `<BACKEND_PORT>` with the port that the Spring Application listens to. In the applciation I use, this is port `9090`.  
If you are using the VMs from the Vagrantfile, then the URL is `http://192.168.56.111`.  

Now open a browser and navigate to the IP address of the machine the angular application is running on.  
If you are using the VMs from the Vagrantfile, then the URL is `http://192.168.56.112`.  
You should see a login page. The username is `admin` and the password is `password`.

# Deployment with Ansible - Docker
Supposing you are in the `devops-ansible` directory, to deploy using Ansible and Docker, run the following commands:

## Docker-compose
```bash
ansible-playbook -l docker-vm playbooks/spring-angular-docker.yaml
```
This playbook will first make sure Docker is installed on the target machine, and then proceed by executing the docker-compose.yaml file.

Now open a browser and navigate to the IP address of the machine that the containers where deployd to.  

For the port, use port `9000`. So for example, navigate to `my-super-app:9000`.  
You should see a login page. The username is `admin` and the password is `password`.
