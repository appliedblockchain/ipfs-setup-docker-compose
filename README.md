# IPFS Cluster Example

An example configuration for running three instances of IPFS as a cluster

## Prerequisites

- [Docker]
- [Docker Compose]

## Usage

Just bring everything up with `docker-compose up`.

[IPFS API] requests can be sent to any of the [IPFS Cluster] instances (port 9095, 9097 or 9099).

If a pin request is sent to any Cluster instance then it will be pinned on all IPFS nodes.

## Info

- The three IPFS instances:
  - Have their default entrypoint script overridden with a modified copy; It has been changed to always initialize new instances with the 'server' profile and make some additional config changes
  - Have their default 'Bootstrap' setting cleared so that they won't attempt to connect to the public network
  - Are run with a common 'swarm.key' file; This makes them run as a private network where each node will only connect to other nodes that have the same key file
  - Have their data directories mounted to the 'ipfs/{1,2,3}' folders in this repo
- The three IPFS-Cluster instances:
  - Have configuration environment variables; These variables will *only* take affect the *first* time an instance is started, and will propagate their values into '$IPFS_PATH/service.json'
  - Have their data directories mounted to the 'cluster/{1,2,3}' folders in this repo


[Docker]:         https://docs.docker.com/
[Docker Compose]: https://docs.docker.com/compose/
[IPFS API]:       https://docs.ipfs.io/reference/api/http/
[IPFS Cluster]:   https://cluster.ipfs.io/documentation/

### Contributors

Dean

#### - [@dpbricher]

Francesco

#### - [@makevoid]


[@dpbricher]: http://github.com/dpbricher
[@makevoid]: http://github.com/makevoid  

### Notes (other prereqs)

You need set CLUSTER_SECRET for the project to work - check CLUSTER_PRIVATEKEY in the docker-compose.yml file.

https://github.com/ipfs/ipfs-cluster-website/blob/master/content/documentation/configuration.md#initializing-a-default-configuration-file

### Other notes

This setup is meant to be ran on both docker-compose, docker swarm (and potentially kube) using the same docker-compose.yml file

On docker swarm we use this command to deploy:

```
docker stack deploy -c docker-compose.yml --with-registry-auth ipfs
```

You can pass `--orchestrator=kubernetes` to docker stack deploy and that will deploy to kubernetes

To try it for example using the `ipfs` package ( https://github.com/ipfs/js-ipfs ) for JS
