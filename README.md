# Docker LEMP Stack #
A Docker-based LEMP stack. Uses the following custom base images, so we don't have to rebuild everything from scratch:

- [monooso/docker-mysql:8.0](https://github.com/monooso/docker-mysql)
- [monooso/docker-nginx:1.17](https://github.com/monooso/docker-nginx)
- [monooso/docker-php:7.3](https://github.com/monooso/docker-php)

In addition to the above, we also spin up a Memcached server, a Redis server, and Mailhog for email testing.

## How to use this repository ##
Fork it, and tweak it as required. GitHub has [comprehensive instructions](https://help.github.com/articles/syncing-a-fork/) on syncing your fork with the upstream repository, but here are the Cliff Notes for the hard of reading:

```
$ git remote add upstream https://github.com/monooso/docker-lemp.git
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
| `USERID`               | `1000`       | The default container user. Run `echo $(id -u)` to determine your user ID. |
| `GROUPID`              | `1000`       | The default container user group. Run `echo $(id -g)` to determine your group ID. |
| `XDEBUG_CONFIG`        |              | Custom xdebug config values which may be used to override (some of) the `xdebug.ini` settings. See the note below for an example of using this configuration value with Docker for Mac. |

Note that the `PATH_*` variables _must not_ include a trailing slash.

## Starting and stopping Docker ##
```bash
$ docker-compose up -d
$ docker-compose down -v
```

## Everyday usage on macOS ##
The following instructions assume that we're running everything on [Docker for Mac](https://docs.docker.com/docker-for-mac/).

### Viewing the website ###
Modify the `server_name` value in the sample Nginx site config file (`config/nginx/sites/app.conf`). For SSL to work correctly, the site _must_ be `<something>.test`.

Once that's done, add an entry to your `/etc/hosts` file, to direct `<something>.test` to `127.0.0.1`.

For example:

```
127.0.0.1    mysite.local.vm
```

### Using Sequel Pro ###
Configure a "standard" Sequel Pro connection, with the following settings:

- Host: `127.0.0.1`
- Username: `root`
- Password: `secret` (unless you changed it in your `.env`)
- Port: `3306`

### Using xdebug ###
Docker for Mac doesn't play nicely with the standard xdebug configuration. To fix this, set the `XDEBUG_CONFIG` variable in your `.env` file as follows:

```
XDEBUG_CONFIG=remote_host=docker.for.mac.localhost
```
