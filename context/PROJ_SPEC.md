# PROJECT SPECIFIC

Stack: PHP 8.4, Laravel, MariaDB, MongoDB

## Makefile Authority

Lifecycle commands:

- `make up`: Boot development environment (all docker containers)
- `make down`: Shut down development environment (leaves containers used by other projects)

Never bypass the Makefile. Extend the Makefile if lifecycle changes are required.

