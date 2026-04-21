# PHP MONOLITH

## Directory Structure

```
<root>/
  Core/
    Component/<ComponentName>/
      Domain/<EntityName>/                  # Pure business logic. Zero framework deps.
      Application/
        Query/<QueryName>/                  # Query handlers
        Repository/
        Listener/                           # Event handlers
        Service/
        UseCase/<UseCaseName>/              # Command handlers
    Port/<ToolName>/                        # Interfaces the core needs
  Infrastructure/<ToolName>/<AdapterName>/  # Implements ports
  Presentation/{Api,Web,Cli}/               # Thin delivery layer
```

`<root>` is `app/` for Laravel projects, otherwise it's `src/`.

Shared Kernel: events, query objects, DTOs, value objects that cross component boundaries.

A component may have sub-components when the domain is large.

The `Core/Component/` grouping is required even for single-component projects — it establishes the boundary that will be enforced when the project grows. Do not flatten `Domain/`, `Application/`, or `Port/` to the source root.

**Legacy:** anything in `app/` outside this structure is legacy. Never place new files in legacy namespaces.

## Dependency Rules

```
                Domain
                  ^
Presentation -> Application -> Port <- Infrastructure
                    |           |          |
                lib/overlay (treated as PHP stdlib)
```

## Message bus

**MANDATORY:** Before doing any work that involves the message bus in any form (dispatching commands, events, queries; writing handlers or listeners; writing tests that interact with the bus), read `vendor/get-e/message-bus/README.ai.md` in full.

### Handler integration tests

- Must dispatch messages through the actual `get-e/message-bus` — never instantiate handlers directly.
