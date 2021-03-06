Planb 
=====
![travis ci](https://travis-ci.org/andrestc/planb-ansible-role.svg?branch=master "Travis CI")
[![Ansible Galaxy](https://img.shields.io/badge/galaxy-andrestc.planb-blue.svg?style=flat)](https://galaxy.ansible.com/andrestc/planb/#details)

This role installs the [planb router](https://github.com/tsuru/planb) as a docker
container on the host.

Role Variables
--------------

``` yml
planb_container_name: "planb"
planb_image: "tsuru/planb"
planb_listen_address: "0.0.0.0:8080"
planb_metrics_address: "0.0.0.0:8090"
planb_read_redis_hos: "localhost"
planb_read_redis_port: 6379
planb_write_redis_host: "localhost"
planb_write_redis_port: 6379
```

Dependencies
------------

Since planb is ran as a docker container on the host, this role depends on [angstwad.docker_ubuntu](https://github.com/angstwad/docker.ubuntu) to setup docker.

Examples
--------

#### Planb with remote read and write redis

This is the most basic way to deploy planb, it will use a remote redis for both read and write operations. 

``` yml
- hosts: servers
  roles:
    - { role: andrestc.planb, planb_write_redis_host: "192.168.50.13",
    planb_read_redis_hos: "192.168.50.13"  }
```
#### Planb with local read redis

This setup will use a local redis as the slave of a remote redis to guarantee better read performance. I recommend [DavidWittman.redis](https://github.com/DavidWittman/ansible-redis) to install the local redis on your server.

``` yml
- hosts: servers
  roles:
    - { role: DavidWittman.redis, redis_slaveof: "192.168.50.13 6379", redis_slave_priority: 0}
    - { role: andrestc.planb, planb_write_redis_host: "192.168.50.13" }
```
Here we used `redis_slave_priority: 0` to prevent this slave from becoming a master.

License
-------

MIT
