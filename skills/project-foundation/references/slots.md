# Slots and library evaluation

## The slots

Every ecosystem needs something in each of these. Fill all of them, or state explicitly why one is skipped.

| Slot | What it answers |
|---|---|
| Runtime / version pinning | Which language/runtime version, and how is it pinned for everyone? |
| Package manager + lockfile | How dependencies are installed reproducibly |
| Formatter | Zero-debate code style, run automatically |
| Linter | Bug patterns, banned APIs, conventions |
| Type checking | Static guarantees beyond the formatter and linter |
| Test runner | Unit/integration tests, and how fast the loop is |
| Boundary enforcement | How architectural rules (who may import whom) are checked by machine |
| Build / run | Dev server, production build, scripts |
| Automation | Pre-commit hook, CI pipeline, coding-agent hooks |
| Env / secrets | How config and secrets are loaded and kept out of git |

## Starting points by ecosystem

These are **starting points to verify, not answers**. Tools and config formats change; check current docs before recommending.

| Slot | TypeScript / JS | Python | Go | Rust |
|---|---|---|---|---|
| Version pinning | `.nvmrc` / `engines` / Volta | `.python-version` (uv) | `go` directive in go.mod | `rust-toolchain.toml` |
| Package manager | pnpm / npm | uv | go modules | cargo |
| Formatter | Prettier or Biome | ruff format | gofmt | rustfmt |
| Linter | ESLint (flat config) or Biome | ruff | go vet + golangci-lint | clippy |
| Types | tsc `strict` | pyright or mypy (strict) | compiler | compiler |
| Tests | Vitest | pytest | go test | cargo test |
| Boundaries | eslint-plugin-boundaries / no-restricted-imports | import-linter | `internal/` packages | crate/module visibility |

For an ecosystem not listed, fill the same slots by researching what its official docs and most-used projects actually use today.

## Library evaluation checklist

Compare 2–3 candidates on the same rows:

- **Fit** — does it solve the actual problem in the brief, or a bigger one?
- **Maintenance** — last release, open-issue response, number of active maintainers
- **Adoption** — usage in well-known projects (not just stars)
- **Docs quality** — can the user learn it without guessing?
- **Types / tooling support** — first-class or bolted on?
- **Weight** — install size, bundle/binary impact, transitive dependencies
- **License** — compatible with how the project is used
- **Exit cost** — how much code would change if we removed it in a year? Prefer libraries you can wrap behind a small interface.

Default to "don't add it" when the standard library or an existing dependency does the job well enough.
