docker-nfs-ganesha
=====================

[![Microbadger Size](https://images.microbadger.com/badges/image/d0whc3r/docker-nfs-ganesha.svg?maxAge=8600)][microbadger]
[![Docker Pulls](https://img.shields.io/docker/pulls/d0whc3r/docker-nfs-ganesha.svg?maxAge=8600)][hub]

[microbadger]: https://microbadger.com/images/d0whc3r/docker-nfs-ganesha
[hub]: https://hub.docker.com/r/d0whc3r/docker-nfs-ganesha/

Docker image providing [NFS-Ganesha](http://nfs-ganesha.github.io/), a user space NFS v3/v4 fileserver.

**Using ganesha 3.0 with centOS 7 image**

## Usage

```bash
$ sudo docker run -d --cap-add SYS_ADMIN --cap-add DAC_READ_SEARCH --name nfs \
 -v /path/to/export:/data/nfs d0whc3r/docker-nfs-ganesha:latest
```

Mount the NFS export:

```bash
$ mkdir -p /mnt/nfs
$ sudo mount -t nfs4 <server-name>:/ /mnt/nfs`
```

## Environment Variables

##### `EXPORT_PATH`
Default: `/data/nfs`
The directory to export.

##### `PSEUDO_PATH`
Default: `/`
NFS4 pseudo path.

##### `EXPORT_ID`
Default: `0`
An identifier for the export, between 0 and 65535.

##### `PROTOCOLS`
Default: `4`
The NFS protocols allowed. One or multiple (comma-seperated) of 3, 4, and 9P.

##### `TRANSPORTS`
Default: `UDP, TCP`
The transport protocols allowed. One or multiple (comma-seperated) of UDP, TCP, and RDMA.

##### `SQUASH_MODE`
Default: `No_Root_Squash`
What kind of user id squashing is performed. No_Root_Squash, Root_Id_Squash, Root_Squash, All_Squash.

##### `GRACELESS`
Default: `true` 
Whether to disable the NFSv4 grace period.

##### `VERBOSITY`
Default: `NIV_EVENT`
Logging verbosity. One of NIV_DEBUG, NIV_EVENT, NIV_WARN.

## Docker compose example

```yaml
version: "3"
services:
  nfs:
    image: d0whc3r/docker-nfs-ganesha
    restart: unless-stopped
    volumes:
      - /path/for/nfs/folder:/data/nfs
    cap_add:
      - SYS_ADMIN
      - SETPCAP
      - DAC_READ_SEARCH
    environment:
      - EXPORT_PATH="/data/nfs"
      - PROTOCOLS="4"
      - TRANSPORTS="TCP, UDP"
    ports:
       - 111:111
       - 111:111/udp
       - 2049:2049
       - 662:662
       - 38465-38467:38465-38467
```
