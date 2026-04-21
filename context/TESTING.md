# TESTING

Canonical testing rules for the monolith. All testing guidance lives here.

## Coverage Target

Minimum 90% test coverage. Use `make coverage` to check. Examine `storage/phpunit/coverage.xml` or `storage/phpunit/report.xml` for uncovered lines.

## Test Commands

All make commands run from host, execute inside the `app` container:

- `make test`: All tests (static analysis + unit tests), as in CI.
- `make unit`: All unit tests. Prepares databases (MariaDB, MongoDB), migrations, seeds.
- `make ut`: Unit tests without DB preparation (faster for repeated runs).
- `make t TEST=ClassName`: Specific test class, no DB prep.
- `make t TEST=ClassName::testMethod`: Specific test method, no DB prep.
- `make t FILE=tests/Path/To/ClassName.php`: Specific test file, no DB prep.

## Static Analysis and Code Quality

- `make static`: All static analysis tools.
- `make stan`: Static analysis.
- `make arch`: Architectural rules.
- `make lint`: Code language syntax.
- `make cs`: Coding standards fixer.
- `make rect`: Code updates (e.g. php 8.2 to 8.4).
- `make fix`: Runs `make cs` and `make rect`.

Before adding or fixing tests, ensure static analysis passes: `make static`.

## Working with Test Databases

Prepared automatically by `make unit`. Manual preparation:

- `APP_ENV='phpunit' make .db-mariadb`: Recreates testing database.
- `APP_ENV='phpunit' make .migrate-mariadb-mongodb`: Runs test migrations.

## Test Naming

Method name describes the scenario, not the implementation:

- `it_emits_transfer_booked_event_when_booking_is_confirmed`
- `it_returns_404_when_booking_does_not_exist`
- `it_rejects_booking_when_pickup_is_in_the_past`

## Test Locations

- Integration tests (boot framework): `tests/Integration/`, mirroring source paths
- Unit tests (no framework boot): `tests/Unit/`, mirroring source paths

| Source | Test |
|--------|------|
| `<src-root>/Core/Component/Invoice/Application/UseCase/CreateInvoice/` | `tests/Integration/Core/Component/Invoice/Application/UseCase/CreateInvoice/` |
| `<src-root>/Presentation/Api/Invoice/` | `tests/Integration/Presentation/Api/Invoice/` |
| `<src-root>/Infrastructure/Database/Invoice/` | `tests/Integration/Infrastructure/Database/Invoice/` |

`<src-root>` is `app/` for Laravel, `src/` otherwise.

## What to Test by Layer

**Domain (`tests/Unit/`)**
- Entity invariants and state transitions
- Value Object construction and equality
- Domain Service computations
- Domain Events are emitted

**Application (`tests/Integration/`)**
- Command Handler: state change, commands emitted, events emitted, side effects, edge cases
- Event Listener: state change, commands emitted, side effects, edge cases
- Query Handler: returns correct read model for given input

**Presentation (`tests/Integration/`)**
- Controller: HTTP status, response shape, dispatched command/query
- Validation: rejected inputs return 422 with correct error shape
- Controllers are tested up to the point where they dispatch a command or event; do not test logic inside dispatched message handlers

**Infrastructure (`tests/Integration/`)**
- Repository: persists and retrieves correctly
- External adapters: sends correct payload, handles error responses

**End-to-end (`tests/Integration/`)**
- At least one smoke test must verify the full pipeline produces expected output with canned/stubbed data

## Test Scope Rules

- All use cases must be tested: all command handlers must have an integration test
- All event listeners must have an integration test
- DB queries are tested as part of encompassing code, except queries with >2 `where` conditions — those must be extracted to a query object & handler, and tested separately
- Tests must cover all happy paths, failure paths, and edge cases
- Every change must be programmatically tested. Write or update a test, then run it.
- Run the minimum number of tests needed for quality and speed.
- Every time a test is updated, run that singular test.

## MUST

- When needing `\GetE\MessageBus\Port\Context\Context` in a test, use `\GetE\MessageBus\Port\Context\MessageBusContext`

## MUST NOT

- Set tests as skipped — either the test runs or it should not exist
- Allow tests to have warnings, notices, or deprecation notices. Use `make ut-debug` to find and fix them.
- Use mocks unless strictly necessary
- Use anonymous classes for test doubles shared across files. Extract named fakes in `tests/Support/`.
- Remove any tests or test files without approval
