---
name: unlock-makefile-commands
description: Reference guide for all Makefile commands available in the Unlock monorepo for managing Docker containers, database operations, testing, and development workflows
metadata:
  trigger_phrases:
    - "makefile commands"
    - "available make commands"
    - "how to reset database"
    - "run tests"
    - "make migrations"
    - "docker compose commands"
---

# Unlock Makefile Commands

This skill documents all available `make` commands for the Unlock application monorepo. These commands manage Docker containers, database operations, testing, migrations, and development workflows.

## Multi-Instance Setup

### `make create_instance`
Create a new development instance with isolated ports and environment.
```bash
make create_instance branch=<branch-name> base=<base-branch> pnpm=<true|false>
```

### `make remove_instance`
Remove a development instance.
```bash
make remove_instance branch=<branch-name>
```

### `make instances`
List all development instances currently set up.
```bash
make instances
```

## Installation & Setup

### `make install`
Run the application first-time installation script.
```bash
make install
```

### `make retrieve_secrets`
Retrieve secrets from AWS Secrets Manager and save to `.env.local` and `.env.dev`.
```bash
make retrieve_secrets
```

## Docker Container Management

### `make build`
Build all containers, or a specific container.
```bash
make build                    # Build all containers
make build app=backend        # Build specific container
```

### `make up`
Run all containers without profiles in detached mode.
```bash
make up
```

### `make up_cx`
Run containers with CX (customer) frontend profile.
```bash
make up_cx
```

### `make up_max`
Run containers with MAX (operations) frontend profile.
```bash
make up_max
```

### `make up_qa`
Run containers with QA profile.
```bash
make up_qa
```

### `make down`
Stop all containers without profiles.
```bash
make down
```

### `make kill`
Stop all containers and networks regardless of profile, with orphan cleanup.
```bash
make kill
```

### `make debug`
Run all containers in debug mode.
```bash
make debug
```

### `make tools`
Run tooling containers (pgadmin, redisinsight, flower).
```bash
make tools
```

### `make up_db`
Start only the database container.
```bash
make up_db
```

### `make logs`
View the last 30 lines of backend logs.
```bash
make logs
```

## Database Operations

### `make reset_db`
**⚠️ WARNING: This will DELETE all data!**
Drop and recreate the database. Requires confirmation.
```bash
make reset_db
```

### `make setup_qa`
Load QA database dump into local environment.
```bash
make setup_qa
```

### `make dbbkp`
Create a database backup to `.dumps/` directory.
```bash
make dbbkp                            # Creates .dumps/default
make dbbkp n="DC-1111-weird-bug"     # Creates .dumps/DC-1111-weird-bug
```

### `make dbrestore`
Restore a database backup from `.dumps/` directory.
```bash
make dbrestore                        # Restores .dumps/default
make dbrestore n="DC-1111-weird-bug"  # Restores .dumps/DC-1111-weird-bug
```

## Django Migrations

### `make makemigrations`
Create new Django migrations.
```bash
make makemigrations              # Create migrations for all apps
make makemigrations app=api      # Create migrations for specific app
```

### `make migrate`
Run Django migrations.
```bash
make migrate                           # Run all pending migrations
make migrate app=api                   # Run migrations for specific app
make migrate app=api migration=0005    # Run specific migration
```

### `make showmigrations`
Show all migrations and their status.
```bash
make showmigrations
```

### `make mergemigrations`
Merge multiple leaf migrations in the migration tree.
```bash
make mergemigrations
```

## Testing

### `make test`
Run backend tests in parallel using pytest.
```bash
make test                                    # Run all tests
make test ARGS="-k test_name"                # Run specific test
make test ARGS="-k test_service"             # Run tests matching pattern
make test ARGS="api/tests/test_views.py"     # Run specific file
```

### `make testx`
Run backend tests, exit on first error/failure (single thread, faster for single tests).
```bash
make testx                                   # Run all tests, stop on first failure
make testx ARGS="-k test_name"               # Run specific test
```

### `make coverage`
Run coverage analysis for the backend.
```bash
make coverage
```

### `make coverage-report`
Generate coverage report.
```bash
make coverage-report
```

### `make coverage-html`
Generate HTML coverage report.
```bash
make coverage-html
```

### `make cx_test`
Run CX (customer) frontend tests.
```bash
make cx_test
```

### `make max_test`
Run MAX (operations) frontend tests.
```bash
make max_test
```

## Frontend Development

### `make cx_lint`
Run linter for CX frontend.
```bash
make cx_lint
```

### `make cx_dev`
Start CX frontend dev server with correct port for this instance.
```bash
make cx_dev
```

### `make max_dev`
Start MAX frontend dev server with Turbopack and correct port for this instance.
```bash
make max_dev
```

### `make cypress_build`
Build Cypress e2e testing image.
```bash
make cypress_build
```

## Django Shell & Commands

### `make sh`
Open a shell in the backend container.
```bash
make sh
```

### `make shell`
Open Django shell_plus (enhanced shell with models auto-imported).
```bash
make shell
```

### `make dbgshell`
Open shell_plus with debugpy installed for debugging.
```bash
make dbgshell
```

### `make superuser`
Create a Django superuser.
```bash
make superuser
```

### `make flush`
Flush (clear) all data from the database.
```bash
make flush
```

### `make run_backfills`
Run database backfill operations.
```bash
make run_backfills
```

## File Permissions

### `make chown`
Change ownership of all files to current user (useful after Docker creates files as root).
```bash
make chown
```

## Celery & Task Management

### `make clear_celery_queues`
Clear all Celery task queues.
```bash
make clear_celery_queues
```

### `make log_segment`
Filter and view Segment tracking events from worker logs. Requires `jq`.
```bash
make log_segment
# Example with filtering:
make log_segment | jq 'select(.event_name == "Notify Customer")'
```

### `make log_succeeded_tasks`
View successfully completed Celery tasks with their arguments. Requires `jq`.
```bash
make log_succeeded_tasks
```

## Easter Egg

### `make sandwich`
🥪 Try it and see!
```bash
make sandwich
```

## Common Workflows

### Starting Development Environment
```bash
# First time
make install
make build
make up_max       # For operations frontend
# or
make up_cx        # For customer frontend

# See logs
make logs
```

### Running Tests During Development
```bash
# Run all tests
make test

# Run specific test file
make test ARGS="api/tests/test_services.py"

# Run tests matching a pattern, stop on first failure
make testx ARGS="-k test_deal_creation"
```

### Database Reset & Migrations
```bash
# Create backup first (optional but recommended)
make dbbkp n="before-reset"

# Reset database
make reset_db

# Run migrations
make migrate

# Create superuser
make superuser
```

### Working with Migrations
```bash
# Check migration status
make showmigrations

# Create new migrations
make makemigrations

# Run migrations
make migrate

# If you have migration conflicts
make mergemigrations
```

### Frontend Development
```bash
# Start MAX (operations) frontend
make max_dev

# Run MAX tests
make max_test

# Start CX (customer) frontend
make cx_dev

# Run CX tests
make cx_test
```

### Debugging
```bash
# Start containers in debug mode
make debug

# Open shell for manual debugging
make shell

# Or with debugpy support
make dbgshell

# View logs
make logs
```

## Tips

- **Use `make testx` for faster feedback** when developing a single test or debugging test failures.
- **Always backup your database** before major operations: `make dbbkp n="descriptive-name"`
- **Use `make kill`** if containers are misbehaving and `make down` doesn't work.
- **The `.env` file** controls which ports your instance uses (multi-instance setup).
- **Check `make instances`** to see all running development instances.
- **Frontend dev servers** (`make cx_dev` and `make max_dev`) run outside Docker for faster hot-reload.

## Environment Variables

Most commands respect environment variables from `.env` file:
- `DB_PORT` - Database port (default: 2345)
- `CX_FRONTEND_PORT` - Customer frontend port (default: 3000)
- `MAX_FRONTEND_PORT` - Operations frontend port (default: 3001)

## Requirements

- Docker & Docker Compose
- For some commands: `jq` (JSON processor)
- For database backup/restore: PostgreSQL client (`psql`, `pg_dump`)
- For frontend dev: Node.js/npm (use fnm or nvm for version management)
