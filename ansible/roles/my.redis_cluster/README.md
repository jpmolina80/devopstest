# Ansible Role: Redis Cluster

Installs [Redis](http://redis.io/) on Linux.

## Role Variables
Available variables are listed below, along with default values (see `defaults/main.yml`):

Path to project directory 

    project_directory: /data/docker/redis
    
Docker image tag
 
    redis_docker_tag: 5.0.7

Docker image redis-server

    redis_server_image: bitnami/redis

Docker image redis-sentinel

    redis_sentinel_image: bitnami/redis-sentinel

Set dns name or ip address to define master node
 
    redis_cluster_master: {{ groups['redis_cluster'][0] }}


This project uses docker `host` networking.


## Dependencies

* docker
* docker-compose

## Example Playbook

    - hosts: redis_cluster
      become: True
      roles:
        - role: redis_cluster

## License

MIT / BSD

## Author Information

This role was created in 2020 by Boris Shestov.
