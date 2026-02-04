# Drupal Demo for /dev/push

Barebones Drupal project for testing the /dev/push Drupal preset.

## Deploy on /dev/push

1) Create a new project and select the **Drupal** preset.
2) Create a database (see [storage docs](https://devpu.sh/docs/basics/storage/)), e.g. `demo-drupal-db`.
3) Set a `HASH_SALT` env var (any long random string).
4) Deploy. When asked for the database in the Drupal installer, choose **SQLite** and set the path to the database path (e.g. `/data/database/demo-drupal-db/db.sqlite`).

Optional: persist public files

1) Create a volume (see storage docs), e.g. `demo-drupal-volume`.
2) Set env var: `VOLUME_PATH=/data/volume/demo-drupal-volume`
3) Add a pre-deploy command for subsequent deploys:
   ```bash
   mkdir -p /app/web/sites/default && ln -sfn "$VOLUME_PATH" /app/web/sites/default/files
   ```

Storage docs: https://devpu.sh/docs/basics/storage/
