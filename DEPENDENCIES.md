# Dependencies

These skills are instruction-only and have no runtime packages, services, or environment variables.

| Skill | Dependency | Purpose | Required? |
|---|---|---|---|
| `context-to-spec` | Codex task environment | Produces the specification. | Yes |
| `context-to-spec` | Independent subagent capability | Performs an extra completeness review when available and authorized. | Optional |
| `hammer-time` | `ponytail:ponytail-review` | Baseline independent phase review. | Yes |

Install `ponytail:ponytail-review` before using `hammer-time`. The two included skills have no dependency on each other at installation time, but `hammer-time` is intended to start only after `context-to-spec` has produced an explicitly approved specification.
