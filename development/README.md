TMS Docker Compose configuration - Developer Environment
========================================================

This folder contains files that can be used to run a development instance of <abbr title="Task Management System">TMS</abbr> with [Docker Compose](https://docs.docker.com/compose/). Please note that only basic features are supported for now, features that require additional software to be installed on the backend server won’t work.

INSTALLATION
------------

1. Clone this repository.
2. Clone the [backend core](https://gitlab.com/tms-elte/backend-core/) and [frontend](https://gitlab.com/tms-elte/frontend-react/) projects.
3. Configure the backend core and frontend projects as necessary. When configuring the backend core, you should set `<db host name>` to `db`. You shouldn’t change the frontend variables `REACT_APP_API_BASEURL` and `REACT_DEV_PROXY` (they define how to access the backend, which won’t change from user to user when using Docker Compose).
4. Create a file named `.env` in this directory containing configuration. See [Configuration](#Configuration) below for details.
5. Start the project by issuing `docker compose up` in this directory (`docker-compose up` if you have Compose V1).
6. Migrate and initialize the database as instructed by the backend core documentation. Whenever the documentation asks you need to run `./yii <parameters>` in the backend core directory, run `docker compose exec backend-core ./yii <parameters>` in this directory instead.
7. Happy hacking!

### Configuration

The following configuration variables are available. Variables without default values should always be specified in the `.env` files, variables that have default values need to be defined only if the default values are inappropriate.

| Name                | Default value       | Description                                                                                                                                               |
|:--------------------|:--------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `BACKEND_CORE_PATH` | `../../backend-core`   | Path to the checked-out backend core project. If relative, it’s relative to the directory containing the `docker-compose.yml` file.                       |
| `FRONTEND_PATH`     | `../../frontend-react` | Path to the checked-out frontend project. If relative, it’s relative to the directory containing the `docker-compose.yml` file.                           |
| `DB_DATABASE`       | (no default)        | Name of the development database, must be the same as `<database name>` in the backend configuration. Used only if no database exists yet.                |
| `DB_USER`           | (no default)        | Name of the database user, must be the same as `<db user>` in the backend configuration. Used if no database exists yet and in phpMyAdmin.                |
| `DB_PASSWORD`       | (no default)        | Password of the of the database user, must be the same as `<db password>` in the backend configuration. Used if no database exists yet and in phpMyAdmin. |

An `.env` file could look like this:
```bash
DB_DATABASE=tms
DB_USER=tms-user
DB_PASSWORD=tms-pwd
```

### Services
The following services are exposed (exposing means that you need to use the Docker host’s IP address or host name to access them):
- The frontend at `0.0.0.0:3000` (i.e. on port 3000 from the local computer and any other computer on the same network)
- [phpMyAdmin](https://www.phpmyadmin.net/) at `127.0.0.1:8080` (i.e. on port 8080 from the local computer only), configured to automatically log in to the database without asking for password
