# ARCHITECTURE

Architecture: Explicit Architecture (DDD + Hexagonal + CQRS), modular monolith

## Priority Order

1. Stability
2. Security
3. Determinism
4. Extensibility
5. Feature growth

## Security

Mandatory:
- No secrets in code. Credentials via `env()` only.
- Never read or reference `.env` files.
- No production DB operations. Scope to local/dev.
- Redact sensitive data in output: `REDACTED`, `user@example.com`, `TOKEN_REDACTED`.
- TLS required
- Restart policies defined
- Resource limits defined
- Health checks required
- No direct public ports (all ingress via reverse proxy)

Forbidden:
- Exposed PostgreSQL ports
- Exposed Redis ports
- Privileged containers
- Host networking
- `latest` image tags
- Hardcoded secrets or default credentials

## Forbidden Patterns

- Outbox pattern
- Anemic Domain models (entities must contain behavior, not just data)
- Business logic in Controllers or Infrastructure
- No raw `DateTime` — use Carbon

## Identifiers

UUID v7, generated in Domain. Never use auto-increment IDs in Domain logic.

## Universal Rules

These rules apply to ALL PHP projects regardless of framework, project size, or directory structure.

- Value Objects over primitives for domain concepts (names, IDs, scores, URLs, etc.)
- All domain classes must be `final` unless explicitly designed for extension
- Port contracts must use typed DTOs — never raw arrays crossing boundaries
- Port interfaces must not expose transport or infrastructure concepts (URLs, headers, connection strings)
- Exceptions that cross layer boundaries must be defined at the port level
- Application services returning data to presentation must return DTOs — never domain entities
- Domain behavior must live on the entity or aggregate that owns the data it operates on
- Dependencies must be explicitly injected — not instantiated inside constructors or factory methods
- Domain model constructors must normalize input (sort collections, trim/lowercase strings)
- Output formatting must be a separate concern from application orchestration
- If an adapter cannot be integration-tested (e.g. real HTTP), use deterministic test doubles
- Infrastructure adapters must validate external data defensively (type-check every field) before mapping to domain types
- Infrastructure adapters must validate URL schemes (require HTTPS) for URLs from external API responses
- Encode external input before interpolating it into URLs, SQL, or shell commands
- Use named test doubles (e.g. `InMemoryFooRepository`, `MapHandlerResolver`) in `tests/Support/` — no anonymous classes for test doubles shared across test files

## Code Quality

- We import classes whenever possible, as opposed to using fully qualified names within the code
- Do not ignore static analysis issues — they must be solved. If not possible, ask the human stakeholder.
- Do not add issues to phparkitect or phpstan baselines — issues must be solved in the codebase.

## Testing Strategy

See `.ai/context/TESTING.md` for comprehensive testing rules.

### MUST
- Run all PHP commands inside the `app` container, through `make run CMD="..."`

### MUST NOT
- Use mocks unless strictly necessary

## Defaults When Uncertain

Prefer: stricter layer separation, explicit modeling, Domain purity.
