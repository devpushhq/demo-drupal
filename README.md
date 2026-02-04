# Drupal Demo for /dev/push

Barebones Drupal project for deploying Drupal on /dev/push.

## Deploy on /dev/push

1. Create a new project and select the **Drupal** preset.
2. Create a database storage (e.g. `demo-drupal-db`).
3. Set environment variables:
   - `DB_PATH=/data/database/demo-drupal-db/db.sqlite`
   - `HASH_SALT=<any-long-random-string>`
4. Set pre-deploy command:
   ```bash
   (./vendor/bin/drush status bootstrap 2>/dev/null | grep -q Successful || ([ -f config/sync/core.extension.yml ] && ./vendor/bin/drush site:install --existing-config -y || ./vendor/bin/drush site:install -y)) && ./vendor/bin/drush deploy
   ```
5. Deploy.

## Optional: Persist uploaded files

By default, uploaded files are lost on redeploy. To persist them:

1. Create a volume storage (e.g. `demo-drupal-files`).
2. Add env var: `VOLUME_PATH=/data/volume/demo-drupal-files`
3. Update pre-deploy command:
   ```bash
   mkdir -p /app/web/sites/default && ln -sfn "$VOLUME_PATH" /app/web/sites/default/files && (./vendor/bin/drush status bootstrap 2>/dev/null | grep -q Successful || ([ -f config/sync/core.extension.yml ] && ./vendor/bin/drush site:install --existing-config -y || ./vendor/bin/drush site:install -y)) && ./vendor/bin/drush deploy
   ```

## Optional: Config management

Export your site configuration and commit it to git:

```bash
./vendor/bin/drush config:export -y
```

This creates YAML files in `config/sync/`. Commit them to git, and `drush deploy` will automatically import them on future deploys.

## Notes

- `settings.php` reads `DB_PATH` and `HASH_SALT` from environment variables.
- Drush handles installation automatically - no web installer needed.
- `drush deploy` runs `updatedb`, `config:import` (if config exists), and `cache:rebuild`.
- If `config/sync/core.extension.yml` exists, `site:install --existing-config` is used.
