# docker-compose-lemp

Docker compose php-fpm nginx mysql sample progect.

Based on:

- [markshust/docker-nginx-phpfpm-percona-alpine](https://github.com/markshust/docker-nginx-phpfpm-percona-alpine.git)
- [khs1994-docker/lnmp](https://github.com/khs1994-docker/lnmp)

## Requirements

- [Docker](https://docs.docker.com/install/)
- [Docker compose](https://docs.docker.com/compose/install/)

## Directories

```shell
├── conf
│   ├── mysql # config for mysql
│   ├── nginx # config for nginx vhost
│   │   └── default.conf
│   └── phpfpm # config for phpfpm
│       ├── zz-docker.conf
│       └── zz-docker.ini
├── docker-compose.yml
├── dockerfile.build-php # custom buildfile for local/php image
├── .env # environment file
├── logs # logs from services
├── private # some private data
├── README.md
├── sql # .sql or .sql.tgz files to import
└── www # for web progect, index.php required
    ├── adminer.php # mysql mangement
    ├── dbtest.php # check phpfpm → mysql connection
    └── phpinfo.php # show phpinfo()
```

## Usage

### start group

```shell
docker-compose --compatibility up -d
```

Options:

- `--compatibility` Compose will attempt to convert keys in v3 files to their non-Swarm equivalent
- `-d`, `--detach` Detached mode; run containers in the background.

### stop group

```shell
docker-compose down
```

## First start or recreate

### update images from registry

```shell
docker-compose pull --include-deps --ignore-pull-failures
```

Options:

- `--ignore-pull-failures` Pull what it can and ignores images with pull failures.
- `--include-deps` Pull services declared as dependencies.

### build local images

```shell
docker-compose build --force-rm --no-cache --pull
```

Options:

- `--force-rm` Always remove intermediate containers.
- `--no-cache` Do not use cache when building the image.
- `--pull` Always attempt to pull a newer version of the image.

### start group with orphans clean and recreates

```shell
docker-compose --compatibility up -d --remove-orphans --force-recreate --always-recreate-deps
```

Options:

- `--compatibility` Compose will attempt to convert keys in v3 files to their non-Swarm equivalent
- `-d`, `--detach` Detached mode; run containers in the background.
- `--force-recreate` Recreate containers even if their configuration and image haven't changed.
- `--always-recreate-deps` Recreate dependent containers.
- `--remove-orphans` Remove containers for services not defined

### stop with orphans clean

```shell
docker-compose down --remove-orphans
```

Options:

- `-v`, `--volumes` Remove named volumes declared in the `volumes` section of the Compose file and anonymous volumes attached to containers.
- `--remove-orphans` Remove containers for services not defined in the Compose file.

## Check orphans and clean it

```shell
docker system prune --all --force --volumes
```

Options:

- `-a`, `--all` Remove all unused images not just dangling ones.
- `-f`, `--force` Do not prompt for confirmation.
- `--volumes` Prune volumes.
