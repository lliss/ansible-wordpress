# ansible-wordpress

An Ansible role for installing WordPress.

## Role Variables

- `wordpress_site_name` - An Nginx site name for the WordPress configuration
- `wordpress_site_repository` - A Git repository for a WordPress installation
- `wordpress_site_repository_depth` - A depth for creating a shallow clone of the repository (helpful for larger repositories, default: `1`)
- `wordpress_site_tag` - A Git tag to checkout after the repository is cloned
- `wordpress_site_root` - An Nginx site root for the WordPress installation
- `wordpress_site_env` - What environment the installation will be running in (default: `development`)
- `wordpress_site_home` - `WP_HOME` in the `wp-config.php` (default: `http://localhost:8080`)
- `wordpress_site_siteurl` - `WP_SITEURL` in the `wp-config.php` (default: `http://localhost:8080`)
- `wordpress_site_local_dev` - `WP_LOCALDEV` in the `wp-config.php` (default: `true`)
- `wordpress_site_db_name` - MySQL database name for the WordPress installation (default: `wordpress`)
- `wordpress_site_db_user` - MySQL user name for the WordPress installation (default: `username`)
- `wordpress_site_db_password` - MySQL password for the WordPress installation (default: `password`)
- `wordpress_site_db_host` - MySQL host for the WordPress installation (default: `localhost`)
- `wordpress_site_auth_key` - `AUTH_KEY` in the `wp-config.php`
- `wordpress_site_secure_auth_key` - `SECURE_AUTH_KEY` in the `wp-config.php`
- `wordpress_site_logged_in_key` - `LOGGED_IN_KEY` in the `wp-config.php`
- `wordpress_site_nonce_key` - `NONCE_KEY` in the `wp-config.php`
- `wordpress_site_auth_salt` - `AUTH_SALT` in the `wp-config.php`
- `wordpress_site_secure_auth_salt` - `SECURE_AUTH_SALT` in the `wp-config.php`
- `wordpress_site_logged_in_salt` - `LOGGED_IN_SALT` in the `wp-config.php`
- `wordpress_site_nonce_salt` - `NONCE_SALT` in the `wp-config.php`
- `wordpress_env` - Mapping of environmental variables for the WordPress installation

## Example Playbook

It is important to note that this role expects the underlying `wp-config.php` file to pull settings from the environment (using `getenv()`):

```php
define('DB_NAME', getenv('WP_DB_NAME'));
define('DB_USER', getenv('WP_DB_USER'));
define('DB_PASSWORD', getenv('WP_DB_PASSWORD'));
```

`wordpress_env` maps most of the defaults that are necessary for a base `wp-config.php`, but it is also necessary to whitelist the same environmental variables in PHP-FPM:

```yaml
php_fpm_env:
  - { name: "WP_ENV", value: "$WP_ENV" }
  - { name: "WP_HOME", value: "$WP_HOME" }
  - { name: "WP_SITEURL", value: "$WP_SITEURL" }
  - { name: "WP_LOCAL_DEV", value: "$WP_LOCAL_DEV" }
  - { name: "WP_DB_NAME", value: "$WP_DB_NAME" }
  - { name: "WP_DB_USER", value: "$WP_DB_USER" }
  - { name: "WP_DB_PASSWORD", value: "$WP_DB_PASSWORD" }
  - { name: "WP_DB_HOST", value: "$WP_DB_HOST" }
  - { name: "WP_AUTH_KEY", value: "$WP_AUTH_KEY" }
  - { name: "WP_SECURE_AUTH_KEY", value: "$WP_SECURE_AUTH_KEY" }
  - { name: "WP_LOGGED_IN_KEY", value: "$WP_LOGGED_IN_KEY" }
  - { name: "WP_NONCE_KEY", value: "$WP_NONCE_KEY" }
  - { name: "WP_AUTH_SALT", value: "$WP_AUTH_SALT" }
  - { name: "WP_SECURE_AUTH_SALT", value: "$WP_SECURE_AUTH_SALT" }
  - { name: "WP_LOGGED_IN_SALT", value: "$WP_LOGGED_IN_SALT" }
  - { name: "WP_NONCE_SALT", value: "$WP_NONCE_SALT" }
```

See the [examples](./examples/) directory for more details.