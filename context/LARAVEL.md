# Laravel Guidelines

## Conventions

- Follow all existing code conventions. When creating or editing a file, check sibling files for structure, approach, and naming.
- Check for existing components to reuse before writing a new one.
- Use APIs from the installed package versions in `composer.json`.

## Application Structure

- Do not change application dependencies without approval.
- Never call `env()` outside of `config/*.php` files. All environment-based values must be read via `Config::` in application code.
- Config files live in `config/` at the package root and are published via `$this->publishes(...)` in the service provider's `boot()` method.

## Do Things the Laravel Way

- Use `php artisan make:` commands to create new files (migrations, controllers, models, etc.).
- Use `php artisan make:class` for generic PHP classes.
- Pass `--no-interaction` to all Artisan commands. Pass correct `--options` for desired behavior.
- Use `php artisan list` to discover commands and `php artisan [command] --help` for parameters.

## Database

- Use Eloquent relationship methods with return type hints. Prefer relationships over raw queries or manual joins.
- Avoid `DB::`; prefer `Model::query()`.
- Prevent N+1 query problems by using eager loading.
- Use Laravel's query builder for very complex operations.
- When modifying a column, the migration must include all previously defined attributes. Otherwise, they will be dropped.
- Laravel 12 allows limiting eagerly loaded records natively: `$query->latest()->limit(10);`.

### Models

- Casts should be set in a `casts()` method rather than `$casts` property. Follow existing model conventions.
- When creating new models, create useful factories and seeders too.

### APIs & Eloquent Resources

- Default to Eloquent API Resources and API versioning, unless existing routes follow a different convention.

## Controllers & Validation

- Create Form Request classes for validation rather than inline validation. Include rules and custom error messages.
- Check sibling Form Requests for array vs string based validation rules.

## Authentication & Authorization

- Use Laravel's built-in features (gates, policies, Sanctum, etc.).

## URL Generation

- Prefer named routes and the `route()` function.

## Testing

- Use model factories. Check if factory has custom states before manually setting up models.
- Faker: Use `$this->faker->word()` or `fake()->randomDigit()`. Follow existing conventions.
- Use `php artisan make:test [options] {name}` for feature tests, `--unit` for unit tests.
- If you see a test using "Pest", convert it to PHPUnit.

## Debugging

- Use `database-query` tool when only reading from the database.
- Use `database-schema` tool to inspect table structure before writing migrations or models.
- Run `php artisan tinker --execute "your code here"` for PHP debugging.
- Run `php artisan config:show [key]` to read configuration values.
- Run `php artisan route:list` to inspect routes.

## Laravel Boost (when MCP is available)

- Laravel Boost is an MCP server with tools designed for this application.
- Use the `get-absolute-url` tool when sharing project URLs.
- Use `browser-logs` tool to read recent browser logs, errors, and exceptions.
- Use `search-docs` tool before trying other approaches when working with Laravel or ecosystem packages. It returns version-specific documentation.
  - Use multiple, broad, simple, topic-based queries: `['rate limiting', 'routing rate limiting', 'routing']`
  - Do not add package names to queries — package info is already shared.
  - Search syntax: simple words (auto-stemming), multiple words (AND), quoted phrases (exact), mixed queries, multiple queries array.

## Laravel 12

- This project upgraded from Laravel 10 without migrating to the new streamlined file structure. This is fine. Follow the existing Laravel 10 structure.

### Laravel 10 Structure

- Middleware: `app/Http/Middleware/`
- Service providers: `app/Providers/`
- No `bootstrap/app.php` configuration:
  - Middleware registration: `app/Http/Kernel.php`
  - Exception handling: `app/Exceptions/Handler.php`
  - Console commands/schedule: `app/Console/Kernel.php`
  - Rate limits: `RouteServiceProvider` or `app/Http/Kernel.php`

## Vite Error

If "ViteException: Unable to locate file in Vite manifest" — run `yarn run build` or ask user to run `yarn run dev` / `composer run dev`.
