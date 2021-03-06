---
sidebarDepth: 3
---

# Docker Compose

[Docker Compose](https://docs.docker.com/compose/) 是官方提供的编排工具，功能强大，简单易用。  

本章不再重复介绍 Docker Compose 的细节，而是集中几个重要的知识点进行备忘说明。

## 命令

```
$docker-compose -h

Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [--profile <name>...] [options] [--] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file
                              (default: docker-compose.yml)
  -p, --project-name NAME     Specify an alternate project name
                              (default: directory name)
  --profile NAME              Specify a profile to enable
  -c, --context NAME          Specify a context name
  --verbose                   Show more output
  --log-level LEVEL           Set log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --ansi (never|always|auto)  Control when to print ANSI control characters
  --no-ansi                   Do not print ANSI control characters (DEPRECATED)
  -v, --version               Print version and exit
  -H, --host HOST             Daemon socket to connect to

  --tls                       Use TLS; implied by --tlsverify
  --tlscacert CA_PATH         Trust certs signed only by this CA
  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
  --tlskey TLS_KEY_PATH       Path to TLS key file
  --tlsverify                 Use TLS and verify the remote
  --skip-hostname-check       Don't check the daemon's hostname against the
                              name specified in the client certificate
  --project-directory PATH    Specify an alternate working directory
                              (default: the path of the Compose file)
  --compatibility             If set, Compose will attempt to convert keys
                              in v3 files to their non-Swarm equivalent (DEPRECATED)
  --env-file PATH             Specify an alternate environment file

Commands:
  build              Build or rebuild services
  config             Validate and view the Compose file
  create             Create services
  down               Stop and remove resources
  events             Receive real time events from containers
  exec               Execute a command in a running container
  help               Get help on a command
  images             List images
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pull service images
  push               Push service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  top                Display the running processes
  unpause            Unpause services
  up                 Create and start containers
  version            Show version information and quit
```

## 规范

Docker Compose 文件用于创建互相关联的容器服务，下面是一个典型的 Compose 文件范例。  

```
version: "3.9"  # optional since v1.27.0
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

Compose 的文件规范请参考官方文档：[The Compose Specification](https://github.com/compose-spec/compose-spec/blob/master/spec.md)

### version

version 参数表示使用哪个版本的 compose 语法，一般是向下兼容。  

## 常见问题

#### 如何给容器赋予宿主机的 root 权限？

```
privileged: true
```

#### 容器中哪些文件夹可以被挂载？

有待进一步研究

#### Compose 如何运行个性化的命令？

有三种运行个性化命令的方式：

* command 实现，例如：command: bundle exec thin -p 3000
* entrypoint 实现，例如：entrypoint: /code/entrypoint.sh
* 多容器共享数据卷实现
  ```
    superset-init:
    image: *superset-image
    container_name: superset_init
    command: ["/app/docker/docker-init.sh"]
    env_file: docker/.env-non-dev
    depends_on: *superset-depends-on
    user: "root"
    volumes: *superset-volumes

  superset-worker:
    image: *superset-image
    container_name: superset_worker
    command: ["/app/docker/docker-bootstrap.sh", "worker"]
    env_file: docker/.env-non-dev
    restart: unless-stopped
    depends_on: *superset-depends-on
    user: "root"
    volumes: *superset-volumes
  ```

#### 是否可以在 Compose 文件中定义一个公用的网络？

可以（仅 3.1 以上可用），代码如下：

```
networks:
  default:
      name: "apps"
```

#### 不同网络的容器是否可以通过主机名互联？

待研究