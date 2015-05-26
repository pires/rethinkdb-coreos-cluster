# rethinkdb-coreos-cluster
[RethinkDB](http://rethinkdb.com/) clustering made easy with [CoreOS](https://coreos.com/), [etcd2](https://coreos.com/etcd/) and [Docker](https://www.docker.com/).

RethinkDB clustering lacks clear and simple discovery support. But [CoreOS makes it really simple, by leveraging on the distributed keystore, `etcd`](https://coreos.com/docs/cluster-management/setup/cluster-discovery/).

>CoreOS uses etcd, a service running on each machine, to handle coordination between software running on the cluster. For a group of CoreOS machines to form a cluster, their etcd instances need to be connected.
>
>A discovery service, https://discovery.etcd.io, is provided as a free service to help connect etcd instances together by storing a list of peer addresses, metadata and the initial size of the cluster under a unique address, known as the discovery URL. You can generate them very easily:
>
>
>The discovery URL can be provided to each CoreOS machine via cloud-config, a minimal config tool that's designed to get a machine connected to the network and join the cluster.

This repository provides one [`cloud-config`](https://coreos.com/docs/cluster-management/setup/cloudinit-cloud-config/) for you, but it will need some preparation in order to use a unique token for cluster discovery, as described above.

```
./setup
```

The output should be something like this:

```
$ ./setup
Retrieving discovery URL from discovery.etcd.io..
Discovery URL: [https://discovery.etcd.io/047a15e89b8d6c471df362c47f45525a]
Preparing cloud-config..
Done. Please use temp/rethinkdb.yaml cloud-config for provisioning your RethinkDB cluster nodes.
```

You should now have a valid `cloud-config` with a unique discovery URL at `./temp/rethinkdb.yaml`. Use it for provisioning your nodes.

### Notes

According to the CoreOS documentation, this `cloud-config` will work only for the providers detailed below.

> The $private_ipv4 and $public_ipv4 substitution variables referenced in other documents are only supported on Amazon EC2, Google Compute Engine, OpenStack, Rackspace, DigitalOcean, and Vagrant.