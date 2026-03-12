# Go Backend Skills

Production-grade agent skills for Go backend development.

## Skills

### `go-backend-init`

Initialize a new Go backend repository with battle-tested engineering setup:

- **Uber FX** dependency injection with layered architecture
- **Ent ORM** for type-safe database operations (PostgreSQL)
- **gRPC + Proto** service contracts
- **golangci-lint v2** with curated linter config
- **GitHub Actions CI** (lint, test, nilaway, autofix)
- **Multi-stage Dockerfile** with build caching
- **VSCode** settings + recommended extensions
- **.env-based config** (no YAML config files)
- **Renovate** for automated dependency updates
- **EditorConfig + cspell** for consistent formatting

## Install

```bash
npx skills add luoling8192/go-backend-skills
```

Or install a specific skill:

```bash
npx skills add luoling8192/go-backend-skills --skill go-backend-init
```

## Usage

After installation, invoke the skill in your AI coding agent:

```
/go-backend-init
```

The skill will guide you through creating a new Go backend project with all the engineering scaffolding.

## License

MIT
