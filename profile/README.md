# Ensemble Edge

**AI orchestration, edge-native. Built exclusively for Cloudflare.**

Build AI workflows that you actually control. No proprietary platforms, no vendor lock-in, no serverless sprawl. Just Git, YAML, and Cloudflare Workers—with zero cold starts across 300+ global locations.

Not cloud-agnostic. Purpose-built for Cloudflare's edge infrastructure: Workers, KV, D1, R2, Durable Objects, Queues, Vectorize, and Workers AI.

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
edgit tag create invoice-schema v1.0.0

# Deploy optimal versions from different timelines
edgit tag set extraction-prompt prod v0.1.0
edgit tag set company-agent prod v3.0.0
edgit tag set invoice-schema prod v1.0.0
edgit push --tags --force

# Instant rollback
edgit tag set extraction-prompt prod v0.1.0
edgit push --tags --force  # < 50ms globally
```

**No merge conflicts. No monorepo versioning. No JSON state files.**

Edgit creates and manages git tags. That's it. GitHub Actions handles deployment. All version data lives in Git tags. Your CI/CD deploys to Cloudflare KV. Every version ever created is instantly accessible at the edge.

[→ Get Started with Edgit](https://docs.ensemble.ai/edgit/getting-started/installation)

---

## Conductor: YAML-Driven Edge Orchestration

Compose AI workflows in YAML. Execute on Cloudflare's global network—300+ locations, zero cold starts, V8 isolates not containers.

```yaml
# ensembles/company-intelligence.yaml
name: company-intelligence

trigger:
  - type: http
    path: /api/company-intel
    methods: [POST]
    public: true

flow:
  - name: fetch-company-data
    agent: fetch-company-data
    input:
      domain: ${input.domain}
    cache:
      ttl: 3600

  - parallel:
    - name: analyze-financials
      agent: analyze-financials

    - name: fetch-competitors
      agent: fetch-competitors

output:
  data: ${fetch-company-data.output}
  analysis: ${analyze-financials.output}
```

**No DAG builders. No UI-driven workflows. No central orchestrator bottleneck.**

Define workflows in Git. Deploy to Cloudflare Workers. State management, caching, and parallelization built in.

[→ Learn About Conductor](https://docs.ensemble.ai/conductor/overview)

---

## Integration: Components → Agents → Ensembles

```yaml
# Version components with Edgit
components:
  extraction-prompt: v1.0.0
  company-analyzer: v2.1.0

# Load as Conductor agents
agents/
  fetch-company-data.yaml
  analyze-financials.yaml
  fetch-competitors.yaml

# Orchestrate in ensembles
ensembles/
  company-intelligence.yaml
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
Multi-layer caching with TTL control. Agent-level cache settings. Cache-first thinking reduces costs and latency.

### Structured Outputs
LLM and API agents produce machine-readable, type-safe output validated via JSON schema.

### Observable by Default
Every execution emits structured logs and metrics. Debugging and transparency are effortless.

### Open by Default
Core tooling (Edgit, Conductor) is open source. Cloud is proprietary—we charge for the UI and managed service, not the runtime.

---

## Status

**Edgit** (active development)
- ✅ Component versioning (prompts, queries, configs, scripts, schemas)
- ✅ Deployment management
- ✅ AI-powered commits
- ✅ Smart detection
- ✅ Component listing (tree, JSON, YAML, table)
- ✅ Discovery tools (scan, detect, patterns)

**Conductor** (production-ready)
- ✅ Core runtime with graph executor
- ✅ State management (immutable, access tracking)
- ✅ Triggers: http, webhook, mcp, email, queue, cron, build, cli (8 types)
- ✅ Operations: think, code, storage, data, http, tools, email, sms, html, pdf, form, queue, docs (13 types)
- ✅ Structured outputs with JSON Schema components
- ✅ Durable Objects (ExecutionState, HITL)
- ✅ Webhooks (inbound/outbound with HMAC signatures)
- ✅ Scheduled execution (cron triggers)
- ✅ Comprehensive test suite (1500+ tests passing)
- ✅ CLI tools (exec, agents, test, docs, history, logs, state, replay, health, config)
- ✅ SDK with client library & testing utilities
- ✅ Observability & logging
- ✅ AI Gateway integration
- ✅ Scoring system for quality metrics

**Cloud** (design phase - managed service with generous free tier)

---

## Quick Start

### Create a Project

```bash
# Launch the wizard (no installation needed)
npx @ensemble-edge/ensemble

# Or create a Conductor project directly
npx @ensemble-edge/ensemble conductor init my-project
cd my-project

# Build and start dev server
pnpm install
pnpm run build
npx wrangler dev --local-protocol http
```

The Ensemble CLI provides access to all tools: **Conductor** (orchestration), **Edgit** (versioning), and **Cloud** (managed platform).

### Version Components with Edgit

```bash
# Initialize Edgit in your project
npx @ensemble-edge/edgit init

# Version a prompt
npx @ensemble-edge/edgit tag create my-prompt v1.0.0

# Deploy to staging
npx @ensemble-edge/edgit tag set my-prompt staging v1.0.0
npx @ensemble-edge/edgit push --tags --force
```

### Deploy to Production

```bash
# Deploy to Cloudflare Workers
npx wrangler deploy

# Execute ensemble
curl https://your-worker.workers.dev/api/hello \
  -H "Content-Type: application/json" \
  -d '{"name": "World"}'
```

### CI/CD Pipelines

For automated environments, use the `-y` flag to skip interactive prompts:

```bash
npx @ensemble-edge/conductor init my-project -y
npx @ensemble-edge/edgit init -y
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

📚 **[Complete Documentation](https://docs.ensemble.ai)** - Comprehensive guides and tutorials

- **[Introduction](https://docs.ensemble.ai/)** - Core concepts and overview
- **[Edgit](https://docs.ensemble.ai/edgit/overview)** - Component versioning system
  - [Installation](https://docs.ensemble.ai/edgit/getting-started/installation)
  - [Versioning Guide](https://docs.ensemble.ai/edgit/guides/versioning-components-agents)
  - [CLI Reference](https://docs.ensemble.ai/edgit/reference/cli-commands)
- **[Conductor](https://docs.ensemble.ai/conductor/overview)** - Edge orchestration framework
  - [Your First Project](https://docs.ensemble.ai/conductor/getting-started/your-first-project)
  - [Operations](https://docs.ensemble.ai/conductor/operations/overview)
  - [Starter Kit](https://docs.ensemble.ai/conductor/starter-kit/overview)
  - [Playbooks](https://docs.ensemble.ai/conductor/playbooks/rag-pipeline)
- **[Cloud](https://docs.ensemble.ai/cloud/overview)** - Managed service (coming soon)
- **[Examples](https://github.com/ensemble-edge/edgit/tree/main/examples)** - Real-world implementations

---

## Contributing

We're building in public. Watch development at:
- **[Edgit Repository](https://github.com/ensemble-edge/edgit)** - Component versioning
- **[Conductor Repository](https://github.com/ensemble-edge/conductor)** - Edge orchestration
- **[Documentation](https://github.com/ensemble-edge/docs)** - Mintlify docs site

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
