# Drupal Demo for /dev/push

Barebones Drupal project for testing the /dev/push Drupal preset.

## Deploy on /dev/push

1) Create a new project and select the **PHP** preset.
2) Create a database (see storage docs), e.g. `demo-drupal-db`.
3) Deploy and complete the Drupal installer.
   - When asked for the database, choose **SQLite** and set the path to:
     `/data/database/demo-drupal-db/db.sqlite`
4) After install, set a `HASH_SALT` env var for the project (any long random string) and re-deploy.

Optional: persist public files

1) Create a volume (see storage docs), e.g. `demo-drupal-volume`.
2) Set env var: `VOLUME_PATH=/data/volume/demo-drupal-volume`
3) Add a pre-deploy command for subsequent deploys:
   ```bash
   mkdir -p /app/web/sites/default \
     && ln -sfn "${VOLUME_PATH%/}" /app/web/sites/default/files \
     && mkdir -p /app/web/sites/default/files/php
   ```

Storage docs: https://devpu.sh/docs/basics/storage/
