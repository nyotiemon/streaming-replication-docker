# Streaming Replication Container Setup for TimescaleDB

Taken from: [source](https://github.com/timescale/streaming-replication-docker)

Modified some of it since it doesnt work on my system

## Running

The containers can either be created through regular Docker commands or through
`Docker Swarm` / `Docker Compose` using the `stack.yml` file.

### Run with Docker

`start_containers.sh` creates a Docker network bridge to connect the primary and
replica then uses the `Dockerfile` to run `replication.sh` against both database
instances.

After ensuring the variables in `primary.env` and `replica.env` match your
desired configuration, simply run:

```bash
./start_containers.sh
```

This will create and run 2 replication-ready containers named
`timescale-primary` and `timescale-replica`.

### Run with Docker Compose

To run with Docker Compose, run:

```bash
docker build -t timescale-replication .
docker-compose -f stack.yml up
```

## Configuration

Configure various replication settings via the `primary.env` and `replica.env`
files. Whether the replication is synchronous or asynchronous (and to what
degree) can be tuned using the `SYNCRHONOUS_COMMIT` variable in `primary.env`.
The setting defaults to `off`, enabling fully asynchronous streaming
replication. The other valid values are `on`, `local`, `remote_write`, and
`remote_apply`. Consult our [documentation][timescale-streamrep-docs] for
further details about trade-offs (i.e., performance vs. lag time, etc).
