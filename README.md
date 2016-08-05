# SETUP "Incirrina" infrastructure
This repository contains the recipes to create a VM and configure environment required for the Incirrina project, the Kwanko data driven counsellor.

## TODOs
- Git clone doesn't due to problem with key/passphrase
- Add frontend role with PHP, nginx, etc
- Add better test files (that uses all installed libs)
2016/08/05
- [Not needed] There's still problem with the user 'recom'
- [Fixed] Flask is not properly installed
- [Fixed]Manage virtual environment (?)

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
    + Add JDBC mysql connector plugin
    + Set database password
    + change import directory and make it writable
    + Start database
- *data_manager* which configures the data manager
    + Create user recom if it doesn't exist 
    + Install pip
    + Install Miniconda
    + Create data_manager environment with numpy
    + Install neo4j-driver, mysql-connector-python, matplotlib
    + Creates public/private key from host ones
    + Create /home/recom/data_manager
    + Copy a test file in /home/recom/data_manager
- *api_server*
    + Create user recom if it doesn't exist 
    + Install pip
    + Install Miniconda
    + Create api_server environment with numpy
    + Install Flask
    + Creates public/private key from host ones
    + Create /home/recom/api_server
- *frontend*
    + To be configured
