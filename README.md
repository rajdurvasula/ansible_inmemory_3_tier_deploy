# 3-Tier application deployment with Ansible In-memory Inventory 

## Pre-requisites:

- An OpenStack environment

- OpenStack Environment with following VMs running:
  - app1
  - app2
  - frontend
  - db

- Metadata for each VM type:
  - app layer (app1, app2):
    - group=apps,deployment_name=QA
  - db layer (db):
    - group=appdbs,deployment_name=QA
  - frontend layer (frontend):
    - group=frontends,deployment_name=QA

## Customization / Configurations:
- If required, use your own SSH keys
- ssh.cfg needs to be updated with your keys
- Change IP addresses according to your environment

- Setup the OpenStack Guest VMs/Instances first:

```
ansible-playbook main.yml -t provision
```

- Setup the 3-Tier app based on in-memory inventory

```
ansible-playbook main.yml -t install_app --ask-vault-pass
```

- Vault password required to use RHEL7 repositories
  - password = passw0rd

## Verification:

- On successful install, the following services should be running on VMs:
  - app1, app2
    - tomcat
  - frontend
    - haproxy
    - haproxy proxies to 8080 port of both app1, app2
    - A default "index.html" is provided for testing
  - PostgreSQL instance with a sample database (empdb)
    - Admin user credentials = postgres/passw0rd
    - Application user credentials = guest1/passw0rd (empdb)

