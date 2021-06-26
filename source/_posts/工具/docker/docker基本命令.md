---
title: docker基本命令
toc: true
date: 2021-04-14 13:49:57
tags: docker
url: docker-command
---

本文将介绍一些docker中的命令与目前过程中的一些技巧,这是自己在学习过程中的一些积累，记录下来，方便查询。

<!-- more -->

## docker 命令大全

## 帮助命令

```bash
docker version --显示docker的版本号
docker info --显示docker的详细信息
docker <命令行> --help 帮助命令
```

docker命令行地址：https://docs.docker.com/engine/reference/run/

## 镜像命令

> docker images 列出镜像

```shell
➜  ~ docker images --help

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --no-trunc        Don't truncate output
  -q, --quiet           Only show image IDs
➜  ~ docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mysql        latest    ab2f358b8612   4 months ago   545MB
zookeeper    latest    ea93faa92337   4 months ago   253MB
redis        latest    84c5f6e03bf0   7 months ago   104MB
nginx        latest    7e4d58f0e5f3   7 months ago   133MB

# 解释
REPOSITORY：镜像的仓库源

TAG：镜像标签

IMAGE ID：镜像id

CREATED：创建时间

SIZE：镜像大小


```

> docker search 搜索镜像

```shell
➜  ~ docker search --help

Usage:  docker search [OPTIONS] TERM

Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
➜  ~ docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10743     [OK]
mariadb                           MariaDB Server is a high performing open sou…   4046      [OK]
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   790                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   532       [OK]
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   87
mysql/mysql-cluster               Experimental MySQL Cluster Docker images. Cr…   81
centurylink/mysql                 Image containing mysql. Optimized to be link…   59                   [OK]
bitnami/mysql                     Bitnami MySQL Docker Image                      50                   [OK]
databack/mysql-backup             Back up mysql databases to... anywhere!         42
deitch/mysql-backup               REPLACED! Please use http://hub.docker.com/r…   41                   [OK]
prom/mysqld-exporter                                                              37                   [OK]
tutum/mysql                       Base docker image to run a MySQL database se…   35
schickling/mysql-backup-s3        Backup MySQL to S3 (supports periodic backup…   29                   [OK]
linuxserver/mysql                 A Mysql container, brought to you by LinuxSe…   27
centos/mysql-56-centos7           MySQL 5.6 SQL database server                   20
circleci/mysql                    MySQL is a widely used, open-source relation…   20
mysql/mysql-router                MySQL Router provides transparent routing be…   19
arey/mysql-client                 Run a MySQL client from a docker container      17                   [OK]
fradelg/mysql-cron-backup         MySQL/MariaDB database backup using cron tas…   12                   [OK]
yloeffler/mysql-backup            This image runs mysqldump to backup data usi…   7                    [OK]
openshift/mysql-55-centos7        DEPRECATED: A Centos7 based MySQL v5.5 image…   6
devilbox/mysql                    Retagged MySQL, MariaDB and PerconaDB offici…   3
ansibleplaybookbundle/mysql-apb   An APB which deploys RHSCL MySQL                2                    [OK]
jelastic/mysql                    An image of the MySQL database server mainta…   1
widdpim/mysql-client              Dockerized MySQL Client (5.7) including Curl…   1                    [OK]
➜  ~ docker search --filter=stars=3000 mysql
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   10743     [OK]
mariadb   MariaDB Server is a high performing open sou…   4046      [OK]

# 重要的可以filter
docker search --filter=stars=3000 mysql
```

> docker pull 拉取

```shell
➜  ~ docker pull mysql
Using default tag: latest
# docker 默认拉取最新的版本号
latest: Pulling from library/mysql
f7ec5a41d630: Pull complete
9444bb562699: Pull complete
6a4207b96940: Pull complete
181cefd361ce: Pull complete
8a2090759d8a: Pull complete
15f235e0d7ee: Pull complete
d870539cd9db: Pull complete
5726073179b6: Pull complete
eadfac8b2520: Pull complete
f5936a8c3f2b: Pull complete
cca8ee89e625: Pull complete
6c79df02586a: Pull complete
Digest: sha256:6e0014cdd88092545557dee5e9eb7e1a3c84c9a14ad2418d5f2231e930967a38
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
➜  ~ docker pull mysql:5.7
# 指定版本号，拉取版本号的镜像
5.7: Pulling from library/mysql
f7ec5a41d630: Already exists
9444bb562699: Already exists
6a4207b96940: Already exists
181cefd361ce: Already exists
8a2090759d8a: Already exists
15f235e0d7ee: Already exists
d870539cd9db: Already exists
## docker采用分层的思想，把一个完整的镜像拆分，如果之前下载过某些分层，则不需要重新下载
7310c448ab4f: Pull complete
4a72aac2e800: Pull complete
b1ab932f17c4: Pull complete
1a985de740ee: Pull complete
Digest: sha256:e42a18d0bd0aa746a734a49cbbcc079ccdf6681c474a238d38e79dc0884e0ecc
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

> docker rmi -f 删除镜像

```shell
➜  ~ docker rmi --help
Usage:  docker rmi [OPTIONS] IMAGE [IMAGE...]
Remove one or more images
Options:
  -f, --force      Force removal of the image
      --no-prune   Do not delete untagged parents
# 删除指定的镜像
➜  ~docker rmi -f daaaaaaaa
# 递归删除全部镜像
➜  ~docker rmi -f $(docker images -aq)
```

## 容器命令

有了镜像才能有容器

> docker run 运行命令

```shell
➜  ~ docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host list                  Add a custom host-to-IP mapping (host:ip)
  -a, --attach list                    Attach to STDIN, STDOUT or STDERR
      --blkio-weight uint16            Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --blkio-weight-device list       Block IO weight (relative device weight) (default [])
      --cap-add list                   Add Linux capabilities
      --cap-drop list                  Drop Linux capabilities
      --cgroup-parent string           Optional parent cgroup for the container
      --cgroupns string                Cgroup namespace to use (host|private)
                                       'host':    Run the container in the Docker host's cgroup namespace
                                       'private': Run the container in its own private cgroup namespace
                                       '':        Use the cgroup namespace as configured by the
                                                  default-cgroupns-mode option on the daemon (default)
      --cidfile string                 Write the container ID to the file
      --cpu-period int                 Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int                  Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int              Limit CPU real-time period in microseconds
      --cpu-rt-runtime int             Limit CPU real-time runtime in microseconds
  -c, --cpu-shares int                 CPU shares (relative weight)
      --cpus decimal                   Number of CPUs
      --cpuset-cpus string             CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string             MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                         Run container in background and print container ID
      --detach-keys string             Override the key sequence for detaching a container
      --device list                    Add a host device to the container
      --device-cgroup-rule list        Add a rule to the cgroup allowed devices list
      --device-read-bps list           Limit read rate (bytes per second) from a device (default [])
      --device-read-iops list          Limit read rate (IO per second) from a device (default [])
      --device-write-bps list          Limit write rate (bytes per second) to a device (default [])
      --device-write-iops list         Limit write rate (IO per second) to a device (default [])
      --disable-content-trust          Skip image verification (default true)
      --dns list                       Set custom DNS servers
      --dns-option list                Set DNS options
      --dns-search list                Set custom DNS search domains
      --domainname string              Container NIS domain name
      --entrypoint string              Overwrite the default ENTRYPOINT of the image
  -e, --env list                       Set environment variables
      --env-file list                  Read in a file of environment variables
      --expose list                    Expose a port or a range of ports
      --gpus gpu-request               GPU devices to add to the container ('all' to pass all GPUs)
      --group-add list                 Add additional groups to join
      --health-cmd string              Command to run to check health
      --health-interval duration       Time between running the check (ms|s|m|h) (default 0s)
      --health-retries int             Consecutive failures needed to report unhealthy
      --health-start-period duration   Start period for the container to initialize before starting health-retries countdown (ms|s|m|h) (default 0s)
      --health-timeout duration        Maximum time to allow one check to run (ms|s|m|h) (default 0s)
      --help                           Print usage
  -h, --hostname string                Container host name
      --init                           Run an init inside the container that forwards signals and reaps processes
  -i, --interactive                    Keep STDIN open even if not attached
      --ip string                      IPv4 address (e.g., 172.30.100.104)
      --ip6 string                     IPv6 address (e.g., 2001:db8::33)
      --ipc string                     IPC mode to use
      --isolation string               Container isolation technology
      --kernel-memory bytes            Kernel memory limit
  -l, --label list                     Set meta data on a container
      --label-file list                Read in a line delimited file of labels
      --link list                      Add link to another container
      --link-local-ip list             Container IPv4/IPv6 link-local addresses
      --log-driver string              Logging driver for the container
      --log-opt list                   Log driver options
      --mac-address string             Container MAC address (e.g., 92:d0:c6:0a:29:33)
  -m, --memory bytes                   Memory limit
      --memory-reservation bytes       Memory soft limit
      --memory-swap bytes              Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int          Tune container memory swappiness (0 to 100) (default -1)
      --mount mount                    Attach a filesystem mount to the container
      --name string                    Assign a name to the container
      --network network                Connect a container to a network
      --network-alias list             Add network-scoped alias for the container
      --no-healthcheck                 Disable any container-specified HEALTHCHECK
      --oom-kill-disable               Disable OOM Killer
      --oom-score-adj int              Tune host's OOM preferences (-1000 to 1000)
      --pid string                     PID namespace to use
      --pids-limit int                 Tune container pids limit (set -1 for unlimited)
      --platform string                Set platform if server is multi-platform capable
      --privileged                     Give extended privileges to this container
  -p, --publish list                   Publish a container's port(s) to the host
  -P, --publish-all                    Publish all exposed ports to random ports
      --pull string                    Pull image before running ("always"|"missing"|"never") (default "missing")
      --read-only                      Mount the container's root filesystem as read only
      --restart string                 Restart policy to apply when a container exits (default "no")
      --rm                             Automatically remove the container when it exits
      --runtime string                 Runtime to use for this container
      --security-opt list              Security Options
      --shm-size bytes                 Size of /dev/shm
      --sig-proxy                      Proxy received signals to the process (default true)
      --stop-signal string             Signal to stop a container (default "SIGTERM")
      --stop-timeout int               Timeout (in seconds) to stop a container
      --storage-opt list               Storage driver options for the container
      --sysctl map                     Sysctl options (default map[])
      --tmpfs list                     Mount a tmpfs directory
  -t, --tty                            Allocate a pseudo-TTY
      --ulimit ulimit                  Ulimit options (default [])
  -u, --user string                    Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                  User namespace to use
      --uts string                     UTS namespace to use
  -v, --volume list                    Bind mount a volume
      --volume-driver string           Optional volume driver for the container
      --volumes-from list              Mount volumes from the specified container(s)
  -w, --workdir string                 Working directory inside the container
```

下面我们拿几个常用的参数解释

```shell
--name 指定运行容器的名字
-d 后台方式
-it 使用交互方式 进入容器查看
-p 指定容器的端口
-P 随机指定端口
```

测试一下

```shell
➜  ~ docker run --name centos1 -itd 300e315adb2f
c1c9a9a26dd33a6392487db1f2a7c960332eac309413a4fa82e9d9f30b4ec3fe
➜  ~ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
c1c9a9a26dd3   300e315adb2f   "/bin/bash"   5 seconds ago   Up 4 seconds             centos1
➜  ~ docker exec -it centos1 /bin/bash
[root@c1c9a9a26dd3 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@c1c9a9a26dd3 /]# pwd
/
[root@c1c9a9a26dd3 /]# whereis nginx
nginx:
```

> docker ps 列出运行的容器

```shell
➜  ~ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED        STATUS        PORTS     NAMES
c1c9a9a26dd3   300e315adb2f   "/bin/bash"   21 hours ago   Up 21 hours             centos1
➜  ~ docker ps -a
CONTAINER ID   IMAGE              COMMAND                  CREATED        STATUS                      PORTS                                                   NAMES
c1c9a9a26dd3   300e315adb2f       "/bin/bash"              21 hours ago   Up 21 hours                                                                         centos1
b20cb371f580   d1165f221234       "/hello"                 4 days ago     Exited (0) 4 days ago                                                               boring_lederberg
056de2c653a8   ab2f358b8612       "docker-entrypoint.s…"   3 months ago   Exited (255) 3 months ago   0.0.0.0:3306->3306/tcp, 33060/tcp                       mysql
5394243f1574   zookeeper:latest   "/docker-entrypoint.…"   4 months ago   Exited (255) 4 days ago     2888/tcp, 3888/tcp, 8080/tcp, 0.0.0.0:12181->2181/tcp   zk1
54a9912b99e1   redis:latest       "docker-entrypoint.s…"   6 months ago   Exited (255) 6 months ago   0.0.0.0:36379->6379/tcp                                 redis-slaver2
2ce7cec87780   redis:latest       "docker-entrypoint.s…"   6 months ago   Exited (255) 3 months ago   0.0.0.0:16379->6379/tcp                                 redis-master
51606ec8b03a   redis:latest       "docker-entrypoint.s…"   6 months ago   Exited (255) 5 weeks ago    0.0.0.0:26379->6379/tcp                                 redis-slaver1
4113a977a62f   nginx:latest       "/docker-entrypoint.…"   6 months ago   Exited (255) 6 months ago   0.0.0.0:80->80/tcp                                      nginx-load-balance
c99db3188f9a   9b9cb95443b5       "/bin/echo 'Hello wo…"   6 months ago   Exited (0) 6 months ago                                                             dreamy_shockley
1522138f775a   9b9cb95443b5       "/bin/echo 'Hello wo…"   6 months ago   Exited (0) 6 months ago                                                             pensive_cartwright
c82cd778a8bc   9b9cb95443b5       "/bin/echo 'Hello wo…"   6 months ago   Exited (0) 6 months ago                                                             flamboyant_nightingale
```

> docker rm 容器id（删除容器）

```shell
docker rm -f 容器id
docker rm 容器id
```

> 容器的其他命令

```shell
docker start 容器id
docker restart 重启容器id
docker stop 容器id
docker kill 容器id
```

> docker 查看日志

```shell
docker logs  -tf -n 20 c1c9a9a26dd3
```

> docker 查看元数据

```shell
➜  Shell docker inspect centos1
[
    {
        "Id": "c1c9a9a26dd33a6392487db1f2a7c960332eac309413a4fa82e9d9f30b4ec3fe",
        "Created": "2021-04-14T14:41:47.9653797Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 3193,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-04-16T09:00:21.3049137Z",
            "FinishedAt": "2021-04-16T01:39:43.8417964Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/c1c9a9a26dd33a6392487db1f2a7c960332eac309413a4fa82e9d9f30b4ec3fe/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/c1c9a9a26dd33a6392487db1f2a7c960332eac309413a4fa82e9d9f30b4ec3fe/hostname",
        "HostsPath": "/var/lib/docker/containers/c1c9a9a26dd33a6392487db1f2a7c960332eac309413a4fa82e9d9f30b4ec3fe/hosts",
        "LogPath": "/var/lib/docker/containers/c1c9a9a26dd33a6392487db1f2a7c960332eac309413a4fa82e9d9f30b4ec3fe/c1c9a9a26dd33a6392487db1f2a7c960332eac309413a4fa82e9d9f30b4ec3fe-json.log",
        "Name": "/centos1",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/274eb8c16a6e5e6ac1a68fb60adc3835a4afa20ed7882daac3b0fd928ab5d405-init/diff:/var/lib/docker/overlay2/7963fffc28e92ac68cf658653fffa8d5c6104941d91985b5ebda6a0c78cea3fd/diff",
                "MergedDir": "/var/lib/docker/overlay2/274eb8c16a6e5e6ac1a68fb60adc3835a4afa20ed7882daac3b0fd928ab5d405/merged",
                "UpperDir": "/var/lib/docker/overlay2/274eb8c16a6e5e6ac1a68fb60adc3835a4afa20ed7882daac3b0fd928ab5d405/diff",
                "WorkDir": "/var/lib/docker/overlay2/274eb8c16a6e5e6ac1a68fb60adc3835a4afa20ed7882daac3b0fd928ab5d405/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "c1c9a9a26dd3",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "300e315adb2f",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "49205c904c996ac8534688930c458ed478263ec01a9553322f3f9bfc53502631",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/49205c904c99",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "54be23afbd52e46a0d8570f6f4ff68b2b2b33f186728c161baa1f6f0b3bfa9db",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "6226106a822b92267889ded6757eab78f6165c65aa638e373c3ae472efe73c86",
                    "EndpointID": "54be23afbd52e46a0d8570f6f4ff68b2b2b33f186728c161baa1f6f0b3bfa9db",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

> docker 查看进程

```shell
docker top containt
```

> 进入当前的容器

```shell
docker exec -it centos1 /bin/bash
# 方式二
docker attach centos1

```

> 从容器中copy

```shell
#从docker中往外拷贝文件
docker cp 容器id:/test.txt ./ 
```

> 制作自己的容器

```shell
➜  Shell docker commit -m='我自己的centos' -a='mrhy' c1c9a9a26dd3 centos-my:1.0
sha256:c3349319374d0630c0a70afa4a3bf7de8c31385f40e60b145d786eb1969e97d6
➜  Shell docker images
REPOSITORY     TAG       IMAGE ID       CREATED         SIZE
centos-my      1.0       c3349319374d   2 minutes ago   209MB
mysql          5.7       450379344707   7 days ago      449MB
mysql          latest    cbe8815cbea8   7 days ago      546MB
centos         latest    300e315adb2f   4 months ago    209MB
zookeeper      latest    ea93faa92337   4 months ago    253MB
redis          latest    84c5f6e03bf0   7 months ago    104MB
nginx          latest    7e4d58f0e5f3   7 months ago    133MB
bde2020/hive   latest    a65dc394c508   2 years ago     1.17GB
```

> docker 挂载卷

```shell
docker run -d -p 3306:3306 --name mysql-master -v=/Users/mrhy/environment/mysql/master/config/my.cnf:/etc/my.cnf -v=/Users/mrhy/environment/mysql/master/data:/var/lib/mysql   -e MYSQL_ROOT_PASSWORD=*** --privileged=true mysql:5.7
```

-v 指的挂载当前文件: 容器内部文件





## docker技巧

> docker容器访问宿主机的地址

宿主机地址为：host.docker.internal