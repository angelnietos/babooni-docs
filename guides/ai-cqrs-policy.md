# AI access policy over CQRS

> Applicability: `@base/backend` hexagonal kernel (F33). Companion to
> [adr-00xx-cqrs-nest.md](../adr/adr-00xx-cqrs-nest.md).

## Principle

**AI agents are read-only by default.** The boundary is *structural*, not just a
runtime check: the read-only gateway injects a `QueryBus` only and never a
`CommandBus`, so an agent physically cannot dispatch a write through it.

Write path (`POST /api/ai/command`) exists but the allow-list ships **empty**.

## Registries (multi-contribution)

Domain modules contribute entries via Nest `multi: true` providers. The global
`CqrsInfraModule` merges them into the runtime maps:

| Token | Contribution token | HTTP | Default |
|-------|-------------------|------|---------|
| `AI_QUERY_REGISTRY` | `AI_QUERY_CONTRIBUTION` | `POST /api/ai/query` | merged from domains |
| `AI_COMMAND_REGISTRY` | `AI_COMMAND_CONTRIBUTION` | `POST /api/ai/command` | empty |

### Registering a query (read)

In the domain Nest module:

```ts
{
  provide: AI_QUERY_CONTRIBUTION,
  multi: true,
  useValue: [
    ['clients.list', ListClientsQuery],
    ['clients.get', GetClientQuery],
  ] satisfies AiQueryContribution,
} as Provider,
```

Naming: `<domain>.<verb>` (`list` / `get` / `search`). Query constructors **must**
accept a single `payload` object (`new Ctor(body.payload ?? {})`).

Registered today (examples): `clients.list`, `clients.get`, `audit.logs`,
`users.list`, `users.get`, `settings.list`, `settings.get`, plus list/get for
roles, tenants, projects, billing, inventory.

### Registering a command (write) — explicit only

`AI_COMMAND_REGISTRY` ships **empty**. A command is exposed to AI only when:

1. it is added via `AI_COMMAND_CONTRIBUTION` (allow-list, never wildcard),
2. it is non-destructive by design,
3. the request carries a valid `x-ai-agent-key`, **and**
4. (future) the caller's role maps to that command via `PERMISSIONS.*`.

Until a contribution exists, `POST /api/ai/command` returns `404 Unknown AI command`.

## Authentication

`AiAgentGuard` enforces the agent key:

- If `AI_AGENT_KEY` env is set, the `x-ai-agent-key` header must match exactly
  → otherwise `401 Unauthorized`.
- If unset (development), the gate is open.
- Authorized requests are flagged `req.isAiAgent = true` for downstream policies.

## Expected responses (contract for tests)

| Scenario | Status |
|----------|--------|
| Invalid / missing `x-ai-agent-key` | `401` |
| Query not in `AI_QUERY_REGISTRY` | `404` |
| Command not in `AI_COMMAND_REGISTRY` | `404` |
| Registered query | `200` |

## Out of scope

- Event sourcing / Kafka per command — the existing `EventBus` in
  `clients`/sagas is preserved.
- SaaS CRM (`@saas/*`) uses its own stack.
