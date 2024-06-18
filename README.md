This README does not describe how to use a Jenkins pipeline.
# Hosts
The `ansible_hosts` in `hosts.yaml` should be the same as the hosts in the `.ssh/config` file of the control node (the machine that executes the playbooks).

# Deployment with just Ansible
Supposing you are in the `playbooks` directory, to deploy using just Ansible, run the following commands:

## Postgres
```bash
ansible-playbook -l db postgres.yaml
```

## Spring Application
```bash
ansible-playbook -l backend -e db_url=<DATABASE_URL>:<DATABASE_PORT> spring.yaml
```
Make sure to replace <DATABASE_URL> with the IP address of the machine that the postgres database runs on.
Also, replace <DATABASE_PORT> with the port that the postgres database listens to, usually this is port 5432.

## Angular Application
```bash
ansible-playbook -l frontend -e backend_server_url=<BACKEND_URL>:<BACKEND_PORT> angular.yaml
```
Make sure to replace <BACKEND_URL> with the IP address of the machine that the Spring Application runs on.
Also., replace <BACKEND_PORT> with the port that the Spring Application listens to. In the applciation I use, this is port 9090.

Now open a browser and navigate to <BACKEND_URL>:<BACKEND_PORT>. You should see a login page.
