TMS Docker Compose configuration - Production Environment
=========================================================

This folder contains files that can be used to run a production instance of <abbr title="Task Management System">TMS</abbr> with [Docker Compose](https://docs.docker.com/compose/). Please note that only basic features are supported for now, features that require additional software to be installed on the backend server won’t work.

INSTALLATION
------------

1. Clone this repository.
2. Create a file named `.env` in this directory containing the Docker Compose configuration. See [Configuration](#Configuration) below for details.
3. Create a `config.yml` file in this directory containing the backend configuration, with the contents required by the [backend core](https://gitlab.com/tms-elte/backend-core/-/blob/develop/README.md) project. When configuring the database connection, you should set `<db host name>` to `db`.
4. Add the necceseary frontend runtime configuration to the `.env` file. See [Configuration](#Configuration) below for supported variables.
5. Start the project by issuing `docker compose up` in this directory (`docker-compose up` if you have Compose V1).
6. The database migration will be performed automatically, but you still need to initialize the database as instructed by the [backend core documentation](https://gitlab.com/tms-elte/backend-core/-/blob/develop/README.md#database-migration). Run the following command:  
   `docker compose exec --user www-data backend-core ./yii setup/init`

### Configuration

The following configuration variables are available. Variables without default values should always be specified in the `.env` files, variables that have default values need to be defined only if the default values are inappropriate.

| Name                            | Default value       | Description                                                                                                                                               |
|:--------------------------------|:--------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `DB_DATABASE`                   | (no default)        | Name of the development database, must be the same as `<database name>` in the backend configuration. Used only if no database exists yet.                |
| `DB_USER`                       | (no default)        | Name of the database user, must be the same as `<db user>` in the backend configuration. Used if no database exists yet and in phpMyAdmin.                |
| `DB_PASSWORD`                   | (no default)        | Password of the of the database user, must be the same as `<db password>` in the backend configuration. Used if no database exists yet and in phpMyAdmin. |
| `VITE_LOGIN_METHOD`             | `MOCK`              | Used login method. Possible values: `LDAP`, `MOCK`                                                                                                        |
| `VITE_THEME`                    | `dark`              | UI theme for the frontend.  Possible values: `dark`, `blue`.                                                                                              |
| `VITE_GOOGLE_ANALYTICS_ID` | (empty string)      | Google Analytics (GA4) tracking ID for website monitoring. If empty or undefined, tracking is disabled.                                                   |

An `.env` file could look like this:
```bash
DB_DATABASE=tms
DB_USER=tms-user
DB_PASSWORD=tms-pwd
```

### Persistent storage

The data stored in the database will be kept in a Docker Volume named `tms_db`.  
The files stored on the disk by the backend core will be mounted in an `appdata` directory in this folder.

You may create a [Docker Compose override file](https://docs.docker.com/compose/how-tos/multiple-compose-files/merge/) to change the location of the persistent storage.

### Services
The following services are exposed (exposing means that you need to use the Docker host’s IP address or host name to access them):
- The frontend at `0.0.0.0:3000` (i.e. on port 3000 from the local computer and any other computer on the same network)
- [phpMyAdmin](https://www.phpmyadmin.net/) at `127.0.0.1:8080` (i.e. on port 8080 from the local computer only), configured to automatically log in to the database without asking for password

*Note:* the backend core will be available on the path `0.0.0.0:3000/api`, through a reverse proxy embedded in the frontend container.