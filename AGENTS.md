# AGENTS.md

## Cursor Cloud specific instructions

### Overview
This is **Moodle LMS 5.2dev**, a PHP web application. The web-accessible document root is `public/`.

### System Dependencies (pre-installed in the environment)
- PHP 8.3+ with extensions: pgsql, gd, intl, mbstring, xml, zip, curl, soap, sodium, opcache
- PostgreSQL 16 (database: `moodle`, user: `moodle`, password: `moodle`)
- Node.js 22.x (for Grunt build toolchain)
- Composer (PHP package manager)

### Starting Services
- **PostgreSQL**: `sudo pg_ctlcluster 16 main start`
- **PHP dev server**: `cd /workspace/public && php -S localhost:8080 -t .` (run in background)
- **Admin login**: username `admin`, password `Admin123!`

### Running Lint / Tests / Build
- **ESLint**: `npx grunt eslint` (from repo root)
- **Stylelint**: `npx grunt stylelint` (from repo root)
- **PHPUnit**: `vendor/bin/phpunit --testsuite <suite_name>` (from repo root)
- **PHPUnit init**: `php public/admin/tool/phpunit/cli/init.php` (required after plugin changes)
- **Moodle upgrade** (after adding plugins): `php admin/cli/upgrade.php --non-interactive --allow-unstable`
- **Purge caches**: `php admin/cli/purge_caches.php`

### Key Paths
- Config: `/workspace/config.php` (gitignored; created by CLI installer)
- Data directory: `/var/www/moodledata`
- PHPUnit data: `/var/www/phpu_moodledata`
- Plugins live under `public/` (e.g. `public/question/type/formulas/`)

### Installed Third-Party Plugins
- `qtype_formulas` (Formulas question type) — `public/question/type/formulas/`
- `qbehaviour_adaptivemultipart` — `public/question/behaviour/adaptivemultipart/`

### Gotchas
- Moodle 5.2dev requires `--allow-unstable` flag for CLI install/upgrade.
- PHP `max_input_vars` must be >= 5000; set via `/etc/php/8.3/cli/conf.d/99-moodle.ini`.
- After a fresh install, run `php admin/cli/cron.php` or queue adhoc tasks to complete `mod_qbank` migration (otherwise question bank shows a warning).
- The `en_AU.UTF-8` locale must be installed for PHPUnit (`sudo locale-gen en_AU.UTF-8`).
