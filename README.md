TMS Docker Compose configuration
================================

This repository contains files that can be used to run an instance of <abbr title="Task Management System">TMS</abbr> with [Docker Compose](https://docs.docker.com/compose/).

- For production usage, see the [Production Environment Guide](production/README.md)
- For development, see the [Developer Environment Guide](development/README.md)

### File Permissions and Commands

> ⚠️ Important: Please refer to the [TMS Backend](https://gitlab.com/tms-elte/backend-core) for more details on file permission.

If you use `docker compose exec backend-core ./yii setup/sample`, the created files (e.g. `appdata/uploadedfiles`) will be owned by the user executing the command inside the container (usually `root`). This can cause errors like: `mkdir(): Permission denied`

To avoid this, **you must run commands as the same user the web server uses**, typically `www-data`. For example:

```bash
docker compose exec --user www-data backend-core ./yii setup/sample
```

If you don't have the proper permissions on the `appdata` folder, run this inside the container to fix it:

```
chown -R www-data:www-data /var/www/html/backend-core/appdata