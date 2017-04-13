# Emma: platform for development of application Spark and DockerSwarm
The platform runs on an infra-structure composed by virtual machines and Ansible playbooks are used to create a storage layer, processing layer and [JupyterHub](https://jupyter-notebook.readthedocs.io/en/latest/index.html) services. The storage layer offers two flavors of storage, file-base by [GlusterFS](https://www.gluster.org/) and object-based using [Minio](https://www.minio.io). The processing layer has a [Apache Spark cluster](http://spark.apache.org/) and a [Docker Swarm](https://docs.docker.com/engine/swarm/) sharing the storage instances.

## Infra-structure

The infra-structure, physical place where the platform runs, is composed by a set of virtual machines with the following characteristics:
1. Ubuntu 16.04 OS
2. Public network interface
3. OS disk, 200Mb for software + enough room in /tmp
4. Passwordless login as root with `${HOST_NAME}.key` private key.
5. XFS Partition mounted at /data/local (used for swapfile, GlusterFS brick, Docker root)
6. Python2 to run Ansible tasks

The virtual machines are requested from a [cloud](cloud.md) provider or simply emulated using [Vagrant](https://www.vagrantup.com/) and details on how to do it are described in [vagrant.md](vagrant.md).

For cloud based setup, skip this when deploying to vagrant.
The disk (in example /dev/vdb) for /data/local can be partitioned/formatted/mounted (also sets ups ssh keys for root) with:
```
ansible-playbook -e datadisk=/dev/vdb prepcloud-playbook.yml
```

If a apt is auto updating the playbook will fail. Use following commands to clean on the host:
```
kill <apt process id>
dpkg --configure -a
```

## Infra-structure provision

Servers are provisioned using [Ansible](https://www.ansible.com/), an automation tool for IT infra-structure. The details on how to do it are described in [ansible.md](ansible.md).

In a nutshell tese are the platform's features:

* [GlusterFS](gluster.md)
* [Minio](minio.md)
* [Spark](spark.md) Standalone cluster
* [Docker Swarm](docker.md)
* [JupyterHub](jupyterhub)


## Deployment

An deployment is done using the demos for the Sherlock project.
```
ansible-playbook demo.yml
```
Once deployed, a website is available on http://\<docker-swarm-manager\> (\<docker-swarm-manager\> is defined in the hosts file as described in [ansible.md](ansbile.md).
