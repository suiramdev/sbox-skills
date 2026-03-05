---
name: sbox
description: "Grounded S&Box documentation retrieval from https://sdocs.suiram.dev. Automatically apply for any S&Box game project, including gameplay, entities, components, systems, packages, addons, modding, and C# work (.cs, .csproj, .sln, dotnet). Use before writing, reviewing, refactoring, or debugging S&Box code."
---

# S&Box Grounded Retrieval

Retrieve S&Box APIs and signatures from `https://sdocs.suiram.dev` and use only grounded results.

Treat this skill as the default path for S&Box game projects.

## API Surface

- Base URL: `https://sdocs.suiram.dev`
- Primary search:
  - `GET /api/sbox/search?q=<query>`
  - `POST /api/sbox/search` with JSON body containing `query`
- Fallback search:
  - `GET /api/search?q=<query>`
  - `POST /api/search` with JSON body containing `query`
  - `GET /api/sdk/search?q=<query>`
  - `POST /api/sdk/search` with JSON body containing `query`
- Detail routes:
  - `GET /api/sdk/describe?id=<id>`
  - `GET /api/sdk/get-signature?id=<id>`
- Optional Q&A:
  - `POST /api/sdk/ask`

## Retrieval Workflow

1. Start with `search_sbox_docs` semantics via `/api/sbox/search` for gameplay/modding questions.
2. If results are sparse or missing, query `/api/search` and `/api/sdk/search`.
3. For any candidate API to use in code, resolve its `id` and fetch:
   - `/api/sdk/describe?id=<id>`
   - `/api/sdk/get-signature?id=<id>`
4. Preserve returned signatures exactly (`displaySignature`, `signature`, `sourceSignature`).
5. If multiple near-matches exist, ask for refinement (namespace, type, method overload, parameter types).
6. Use `/api/sdk/ask` only as supporting context, then confirm final APIs with search + signature routes.

## Response Rules

- Never invent S&Box APIs, types, members, namespaces, or signatures.
- Cite grounded identifiers when proposing code (at minimum include API name + signature).
- Prefer APIs with clearer member metadata (`description`, `parameters`, `returnType`, `exampleUsage`) when available.
- Before writing or editing S&Box code, run retrieval first.
- If no grounded match is found, say so explicitly and request a narrower query.

## Query Quality

Use specific, narrow queries first:

- `Sandbox.GameObject`
- `Sandbox.Component Enabled`
- `Sandbox.Services.Players.Overview.Player`
- `OnUpdate override`

Then broaden only if needed.

## Tool Mapping

- `search_sbox_docs` -> `https://sdocs.suiram.dev/api/sbox/search`
- `search_sdk` -> `https://sdocs.suiram.dev/api/search` and `https://sdocs.suiram.dev/api/sdk/search`

Use these mappings even when the user does not explicitly name the skill.
