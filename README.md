# devopstest
Deploy a VirtualBox Machine with Ubuntu Bionic64 using vagrant, and run an ansible playbook to deploy Redis, RabbitMQ and MySQL clusters.

# Sources

Ansible roles used from Ansible Galaxy:
 - geerlingguy.pip: installs and configure PIP
 - geerlingguy.docker: install and configure docker and docker-compose commands
 - johannweging.docker-compose: deploys docker-compose files.
 - elbars.redis_cluster: deploys a redis cluster (customized and renamed to my.redis_cluster)

## Requisites

This project was tested using:
 - macOs Catalatina 10.15.6
 - Ansible 2.9.13
 - Vagrant 2.2.10
 - Python 3.8.5

For the VM, it is recommended to use 4GB of RAM as is defined by default in the Vagrantfile.

## How to run

Just clone or download this repository and run command `vagrant up`


## Variables

Some variables can be defined in the file `./ansible/environments/devopstest/group_vars/mygroup`

    ---
    
    rabbitmq:
      default_user: admin2
      default_pass: admin2
    
    mysql:
      replication_user: replication
      replication_pass: replication
      root_pass: superpassword
      user: user1
      pass: password1
      database: test1



## Ports exposed on Host Machine

The following ports are forwarded to the Host Machine in order to access to the different services of this project from outside of the VM.
 - SSH: TCP/22 (vagrant/vagrant)
 - Redis: TCP/6379
 - RabbitMQ UI: 15672 (default: admin2/admin2)
 - MySQL DB: 3306 (default: user1/password1/test1)


## Redis cluster

This cluster is composed by 5 docker containers:
 - redis_redis_1: master server
 - redis_redis-slave_1: replica server
 - redis_redis-sentinel_1: sentinel server 1
 - redis_redis-sentinel_2: sentinel server 2
 - redis_redis-sentinel_3: sentinel server 3

by default, 2 sentinels servers need to agree about the fact the master is not reachable (REDIS_SENTINEL_QUORUM)

# RabbitMQ cluster

This cluster is composed by 3 docker containers:
 - ansible_rabbit1_1: master server
 - ansible_rabbit2_1: slave (links to master node ansible_rabbit1_1)
 - ansible_rabbit3_1: slave (links to master node ansible_rabbit1_1, ansible_rabbit2_1)

Some variables can be defined in the file `./ansible/environments/devopstest/group_vars/mygroup`

    rabbitmq:
      default_user: admin2
      default_pass: admin2

## MySQL DB

This cluster is composed by 2 docker containers:

 - ansible_mysql-master_1: master server
 - ansible_mysql-slave_1: slave server

Data is persisted in `/data/mysql` 

Some variables can be defined in the file `./ansible/environments/devopstest/group_vars/mygroup`

    mysql:
      replication_user: replication
      replication_pass: replication
      root_pass: superpassword
      user: user1
      pass: password1
      database: test1

## Author
Juan Pablo Molina Arce (jpmolina@gmail.com)
