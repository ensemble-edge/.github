# Ensemble Edge

**Version control meets edge orchestration for AI systems.**

Build AI workflows that you actually control. No proprietary platforms, no vendor lock-in, no serverless sprawl. Just Git, YAML, and Cloudflare Workers.

---

## The Problem

You have 50+ prompts, 20+ agents, and dozens of SQL queries scattered across your codebase. Changing one breaks three others. Rollbacks mean digging through Git history. A/B testing means deployment hell. And every "AI orchestration platform" wants you to give up control for their black box.

**You know there's a better way.**

---

## Edgit: Git-Native Component Versioning

Version every AI component independently. Deploy any combination from any point in history.

```bash
# Each component versions independently
edgit tag create extraction-prompt v1.0.0
edgit tag create company-agent v2.1.0
edgit tag create validation-sql v0.5.0

# Deploy optimal versions from different timelines
edgit deploy set extraction-prompt v0.1.0 --to prod  # Ancient but perfect
edgit deploy set company-agent v3.0.0 --to prod      # Latest stable
edgit deploy set validation-sql v2.5.0 --to prod     # Optimal performance

# Instant rollback
edgit deploy set extraction-prompt v0.1.0 --to prod  # < 50ms globally
```

**No merge conflicts. No monorepo versioning. No JSON state files.**

All version data lives in Git tags. Your CI/CD deploys to Cloudflare KV. Every version ever created is instantly accessible at the edge.

[→ Edgit Documentation](./edgit)

---

## Conductor: YAML-Driven Edge Orchestration

Compose AI workflows in YAML. Execute at 200+ global locations. Sub-50ms cold starts.

```yaml
# ensembles/company-intelligence.yaml
name: company-intelligence

state:
  schema:
    companyData: object
    analysis: object

flow:
  - member: fetch-company-data
    state:
      set: [companyData]
    input:
      domain: ${input.domain}
    cache:
      ttl: 3600

  - parallel:
    - member: analyze-financials
      state:
        use: [companyData]
        set: [analysis]

    - member: fetch-competitors
      state:
        use: [companyData]
```

**No DAG builders. No UI-driven workflows. No central orchestrator bottleneck.**

Define workflows in Git. Deploy to Cloudflare Workers. State management, caching, and parallelization built in.

[→ Conductor Documentation](./conductor)

---

## Integration: Components → Members → Ensembles

```yaml
# Version components with Edgit
components:
  extraction-prompt: v1.0.0
  company-analyzer: v2.1.0

# Load as Conductor members
members:
  - type: Think
    component: extraction-prompt@v1.0.0
    config:
      model: gpt-4
      cache_ttl: 3600

# Orchestrate in ensembles
ensembles:
  - name: company-intel
    members: [fetch, analyze, score]
    deploy: cloudflare-workers
```

Mix optimal component versions. Test combinations locally. Deploy atomically to the edge.

---

## Ensemble Cloud (Future)

Managed service with generous free tier for:

**Prompt & Component Management**
- Visual editor for prompts, SQL, configs
- Version and tag changes with one click
- A/B test and multivariant experiments
- Let analysts iterate without touching code

**Workflow & Deployment**
- Visual workflow builder (React Flow)
- Deploy to production from the UI
- Real-time collaboration across teams
- Observability dashboards

**The Difference:** Your data lives in Git, not our database. We're the UI layer. You own the source of truth.

Think PromptLayer's editing experience, but Git-native instead of proprietary storage.

---

## Architecture Principles

### Edge-First
Cloudflare Workers, KV, D1, R2, and AI Gateway are the primitives. No centralized compute.

### Git-Native
Configuration, orchestration, and versioning live in Git. CLI and SDK are thin layers around Git operations.

### Cache-Central
Multi-layer caching with TTL control. Member-level cache settings. Cache-first thinking reduces costs and latency.

### Structured Outputs
LLM and API members produce machine-readable, type-safe output validated via JSON schema.

### Observable by Default
Every execution emits structured logs and metrics. Debugging and transparency are effortless.

### Open by Default
Core tooling (Edgit, Conductor) is open source. Cloud is proprietary—we charge for the UI and managed service, not the runtime.

---

## Status

**Edgit:** v0.1.8 (active development)
- ✅ Component versioning
- ✅ Deployment management
- ✅ AI-powered commits
- ✅ Smart detection

**Conductor:** v0.0.1 (building now)
- 🚧 Core runtime
- 🚧 State management
- 📋 Scoring system
- 📋 MCP integration

**Cloud:** Design phase (managed service with generous free tier)

---

## Quick Start

### Install Edgit
```bash
npm install -g @ensemble-edge/edgit
cd your-repo
edgit init
```

### Version Components
```bash
edgit tag create my-prompt v1.0.0
edgit deploy set my-prompt v1.0.0 --to staging
```

### Deploy to Edge
```bash
edgit build --target cloudflare
edgit deploy --to cloudflare
```

### Conductor (Coming Soon)
```bash
npm install @ensemble-edge/conductor
conductor init
conductor deploy
```

---

## Why This Exists

Modern AI development looks like this:
1. Ship v1.0.0 of your app
2. Change a prompt
3. Everything becomes v2.0.0
4. Original prompt trapped in Git history
5. Can't A/B test old vs new
6. Can't mix optimal versions
7. Rollback means reverting the entire codebase

**This is insane for systems with 100+ independently evolving components.**

Ensemble Edge treats AI components like they deserve: individually versioned, independently deployable, infinitely composable.

Then orchestrates them at the edge where they execute fast and scale infinitely.

---

## Documentation

- [Edgit Guide](./edgit/README.md) - Component versioning
- [Conductor Guide](./conductor/README.md) - Edge orchestration
- [Integration Guide](./docs/integration.md) - Using both together
- [API Reference](./docs/api.md) - SDK documentation
- [Examples](./examples) - Real-world implementations

---

## Contributing

We're building in public. Watch development at:
- [Edgit Repository](./edgit)
- [Conductor Repository](./conductor)
- [Planning Documents](./.planning)

Issues, PRs, and feedback welcome.

---

## License

- **Edgit:** MIT
- **Conductor:** Apache 2.0
- **Documentation & Examples:** MIT
- **Ensemble Cloud:** Proprietary (managed service)

Open core, closed cloud. Use it, fork it, build on it.

---

Built by engineers who believe AI tooling should be as solid as the infrastructure it runs on.

**No buzzwords. No hand-holding. Just tools that work.**
