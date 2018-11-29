# Morpheus Playbook
---
Morpheus is a Cloud Management Platform (CMP).   It not only integrates with current public and private clouds, but allows you to quickly integrate a more agile approach to automating the full stack. 


**Important Notes:** 
Currently the "percona" flag is a run once and only once configuration.  
RabbitMQ and ElasticSearch  tag can be run multiple times.
You must configure the hosts file with the IPs of the servers you wish to use.  
The playbook is setup to detect the type of OS and completely deploy the particular tag you wish to use.  
MAKE SURE YOU HAVE THE GROUP VARS FILLED OUT - YOU WILL NOT BE SUCCESSFUL WITHOUT THAT BEING FILLED OUT PROPERLY. 

# Types of installs
---
**Distributed HA** - this ansible playbook does have the ability to install all 3 tiers at once with a single command.  Along with that you do have the ability to run each tier independently of each other.

**Distributed Multisite HA** -  

**Multiple frontend with external databases**

**Single Node** - POC mode more options to be developed later. 



# Distributed HA / Distributed Multisite HA - Individual Service Install
---

##Prepare Hosts: 
---
When using this playbook it is assuming that you don't have python installed which is a requirement.  If you have 5 hosts you can get the basic requirements installed by using the following command structure: 

For both CentOS and Ubuntu: 

``time ansible-playbook -i [location of hosts file] morpheus.yml -K --tags 'prepare_hosts' -e "ansible_user=[your_username]"``

The following command runs the following steps: 
- Checks to see if the python is installed. 
- If python is not installed it runs the raw: install python command

It has gather_facts disabled it also has the strategy set to free which means all of them start at once and ansible will try and complete them as fast as possible. 


## Elastic Search Install 
---
The playbook assumes that you forgot the above flag and will automatically start with trying to install python.  If it detects python it will continue. 

We also assume that you have either of the supported versions of Linux.  The playbook will automatically test to see which set of tasks it should run. 

For both CentOS and Ubuntu: 

``time ansible-playbook -i [location of hosts file] morpheus.yml -K --tags 'elastic' -e "ansible_user=[your_username]"``

The following command runs the following items: 
1. Java Install
2. ElasticSearch Install - run an installer that grabs the supported version of ES that is needed for morpheus. 
3. Push ElasticSearch Config - there is a template file that contains the best practices morpheus recommends. 
4. Restart ElasticSearch

## RabbitMq Install:
---
The playbook assumes that you forgot to prepare hosts.  It will first verify python is installed on the nodes. If it detects python is already installed it will continue.  

We also assume that you have either of the supported verions of Linux.  The playbook will automatically test to see which set of tasks need to be run. 

For both CentOS and Ubuntu: 

``time ansible-playbook -i [location of hosts file] morpheus.yml -K --tags 'rabbitmq' -e "ansible_user=[your_username]"``

The command runs the following items: 
1. Updates host files with all of the proper names of all of the participating names. 
2. Install erlang and socat
3. Install RabbitMQ
4. Cluster RabbitMQ nodes
5. Install RabbitMQ Plugins
6. HA config - create vhost and username - finally setting HA queue. 


## Percona Install:
---
The playbook assumes that you forgot to prepare hosts.  It will first verify python is installed on the nodes.  If it detects python is already installed it will continue.

We also assume that you have either of the supported versions of Linux.  The playbook will automatically test to see which set of tasks need to be run. 

For both CentOS and Ubuntu:

``time ansible-playbook -i [location of hosts file] morpheus.yml -K --tags 'percona' -e "ansible_user=[your_username]"``

The command runs the following items: 
1. Prep the host by installing: MYSQL-Python, pip, pymysql
2. Import percona repo and install percona
3. Set a temporary root password. 
4. Set sstuser
5. Configure percona
6. Bootstrap 1st node. 
7. Add nodes to the cluster.
8. Create the database. 


## Placeholder for Frontend install
---



# Distributed HA / Distributed Multisite HA - All in one install
---

PLACE HOLDER FOR AN ALL IN ONE INSTALLER




