# mehta/apt-cacher-ng

# Getting started

## Installation

```bash
docker pull mehta/apt-cacher-ng:latest
```

Alternatively you can build the image yourself, go inside docker-apt-cacher-ng

```bash
make
```

## Quickstart

Start Apt-Cacher NG using:

```bash
docker run -d --restart=always
  --publish 3142:3142 \
  --volume /usr/local/var/docker/apt-cacher-ng:/var/cache/apt-cacher-ng \
  mehta/apt-cacher-ng:latest
```

*Alternatively, you can use the sample [docker-compose.yml](docker-compose.yml)*

## Persistence

For the cache to preserve its state across container shutdown and startup you should mount a volume at `/var/cache/apt-cacher-ng`.

> *The [Quickstart](#quickstart) command already mounts a volume for persistence.*

```bash
mkdir -p /usr/local/var/docker/apt-cacher-ng
```

## Usage

To start using Apt-Cacher NG on your Debian (and Debian based) host, create the configuration file `/etc/apt/apt.conf.d/01proxy` with the following content:

```config
Acquire::HTTP::Proxy "http://172.17.0.2:3142";
Acquire::HTTPS::Proxy "false";
```

Similarly, to use Apt-Cacher NG in you Docker containers add the following line to your `Dockerfile` before any `apt-get` commands.

```dockerfile
RUN echo 'Acquire::HTTP::Proxy "http://172.17.42.1:3142";' >> /etc/apt/apt.conf.d/01proxy \
 && echo 'Acquire::HTTPS::Proxy "false";' >> /etc/apt/apt.conf.d/01proxy
```

## Logs

To access the Apt-Cacher NG logs, located at `/var/log/apt-cacher-ng`, you can use `docker exec`. For example, if you want to tail the logs:

```bash
docker exec -it apt-cacher-ng tail -f /var/log/apt-cacher-ng/apt-cacher.log
```

# Maintenance

## Cache expiry

Using the [Command-line arguments](#command-line-arguments) feature, you can specify the `-e` argument to initiate Apt-Cacher NG's cache expiry maintenance task.

```bash
docker run --name apt-cacher-ng -it --rm \
  --publish 3142:3142 \
  --volume /srv/docker/apt-cacher-ng:/var/cache/apt-cacher-ng \
  mehta/apt-cacher-ng:latest -e
```

CREDITS: It is forked from https://github.com/sameersbn/docker-apt-cacher-ng and modified for OSx.
