# Docker LEMP Stack #
A Docker-based LEMP stack, for use on macOS. Uses the following custom base images, so we don't have to rebuild everything from scratch:

- [monooso/docker-mysql:latest](https://github.com/monooso/docker-mysql)
- [monooso/docker-nginx:latest](https://github.com/monooso/docker-nginx)
- [monooso/docker-php:latest](https://github.com/monooso/docker-php)

In addition to the above, we also spin up a Redis server, and Mailhog for email testing.

## How to use this repository ##
Fork it, and tweak it as required. GitHub has [comprehensive instructions](https://help.github.com/articles/syncing-a-fork/) on syncing your fork with the upstream repository, but here are the Cliff Notes for the hard of reading:

```
$ git remote add upstream https://monooso@bitbucket.org/experience/docker-lemp.git
$ git fetch upstream
$ git checkout master
$ git merge upstream/master
```

## Configuration ##
Duplicate the `.env.example` file, and name it `.env`. Edit the configuration values, as required.

| Key                    | Default      | Description |
|:-----------------------|:-------------|:------------|
| `COMPOSE_PROJECT_NAME` | `newproject` | The `docker-compose` "project name" parameter. |
| `MYSQL_DATABASE`       | `newproject` | The name of the MySQL database which will be created. |
| `MYSQL_USERNAME`       | `dba`        | The MySQL username. |
| `MYSQL_PASSWORD`       | `dba`        | The MySQL password. |
| `MYSQL_ROOT_PASSWORD`  | `secret`     | The MySQL root user password. |
| `PATH_APP_ROOT`        | `./../app`   | The path to the project application code directory. |
| `PATH_CONFIG_ROOT`     | `./config`   | The path to the project config directory. The default config directory include an nginx site config file, which should work for most projects. If you need to customise things, just point the `PATH_CONFIG_ROOT` to a different directory. |
| `PATH_DATA_ROOT`       | `./data`     | The path to the project data directory, for use with MySQL. |
| `PATH_LOGS_ROOT`       | `./logs`     | The path to the project logs data directory. |

Note that the `PATH_*` variables _must not_ include a trailing slash.

## Starting and stopping Docker ##
```bash
$ docker-compose up -d
$ docker-compose down -v
```

## Viewing the website ##
Assuming you're only hosting one site at a time with this stack (the intended use-case), you shouldn't need to mess with your `/etc/hosts`; your site with be available at `http://<anything>.docker`.

If you don't like the `.docker` TLD (Dinghy's default), you can change it by editing your `~/.dinghy/preferences.yml` file:

```
---
:preferences:
  :dinghy_domain: vm
```

## Connecting to the database ##
The following instructions assume that we're running everything within a [Dinghy](https://github.com/codekitchen/dinghy) VM.

### Sequel Pro ###
Determine the I.P. address of the Docker VM, using `dinghy ip`. Then configure a "standard" Sequel Pro connection, with the following settings:

- Host: `<dinghy ip address>`
- Username: `root`
- Password: `secret` (unless you changed it in your `.env`)
- Port: `3306`
