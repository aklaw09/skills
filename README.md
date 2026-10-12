# Codex Skills

Reusable Codex skills for turning product context into an approved specification and delivering the approved work safely.

## Included skills

- `context-to-spec`: turns mixed product context into a traceable product specification. It stops for explicit approval before technical design begins.
- `hammer-time`: takes an approved specification through design review, phased planning, implementation, verification, and review.

## Install

Copy the desired skill folder into your Codex skills directory:

```sh
cp -R context-to-spec hammer-time ~/.codex/skills/
```

Restart or start a new Codex task, then invoke `$context-to-spec` before `$hammer-time`.

## Workflow

```text
Rough context → context-to-spec → approved specification → hammer-time
```

See [DEPENDENCIES.md](DEPENDENCIES.md) for required and optional capabilities.
