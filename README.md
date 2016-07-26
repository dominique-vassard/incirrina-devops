# SETUP "Incirrina" infrastructure
This repository contains the recipes to create a VM and configure environment required for the Incirrina project, the Kwanko data driven counsellor.

## Requirements
Vagrant
Ansible

## TL, DR;
- Clone current repository
- Get into /conf_infra directory
- run `setup_infra.sh`
- Wait until setup is finished (can take up to 15-20 minutes)
- Wait a little bit more (20 s max) for the database to be fully started
- Connect into vm: `vagrant ssh`
- Test that everything works fine: `python /home/recom/data_manager/test_connector.py`
- If everything went fine, you should see: 
```
Anna
Bob
<Path start=3 end=6 size=4>
```

## Description
### setup_infra.sh
This script will:
- create folder ../dev_home/data_manager in which data_manager files should be stored
- create folder ../dev_home/api_server in which api_server files should be stored
- create folder ../dev_home/neo4j in which database data should be stored
- launch `vagrant up` to initialize virtual machine

### Vagrantfile
This file defines the virtual machine.
The following folders will be synced:
- ../dev_home/data_manager -> /home/recom/data_manager
- ../dev_home/api_server -> /home/recom/api_server
- ../dev_home/neo4j -> /var/lib/neo4j/data

### Ansible provisioning
4 roles exist:
- *graph_db*, which configures the db:
    + Install Java 8
    + Install Neo4j globally
    + Add apoc procedures plugin
    + Add JDBC mysql conenctor plugin
    + Set database password
    + Start database
- *data_manager* which configures the data manager
    + Create user recom if it doesn't exist 
    + Install pip
    + Install Miniconda
    + Install Flask
    + Install neo4j-driver
    + Create /home/recom/data_manager
    + Copy a test file in /home/recom/data_manager
- *api_server*
    + Create user recom if it doesn't exist 
    + Install pip
    + Install Flask
    + Create /home/recom/api_server
- *frontend*
    + To be configured
