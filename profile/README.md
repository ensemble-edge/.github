# Ensemble Edge

**Version control meets edge orchestration for AI systems.**

Build AI workflows that you actually control. No proprietary platforms, no vendor lock-in, no serverless sprawl, no duct tape and bailing wire "workflow automation." Just Git, YAML, and Cloudflare Workers. Fast, developer-first, infinite scale.

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

[→ Get Started with Edgit](https://docs.ensemble.ai/getting-started/edgit)

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
  - agent: fetch-company-data
    state:
      set: [companyData]
    input:
      domain: ${input.domain}
    cache:
      ttl: 3600

  - parallel:
    - agent: analyze-financials
      state:
        use: [companyData]
        set: [analysis]

    - agent: fetch-competitors
      state:
        use: [companyData]
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
agents:
  - operation: think
    component: extraction-prompt@v1.0.0
    config:
      model: gpt-4
      cache_ttl: 3600

# Orchestrate in ensembles
ensembles:
  - name: company-intel
    agents: [fetch, analyze, score]
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
Multi-layer caching with TTL control. Agent-level cache settings. Cache-first thinking reduces costs and latency.

### Structured Outputs
LLM and API agents produce machine-readable, type-safe output validated via JSON schema.

### Observable by Default
Every execution emits structured logs and metrics. Debugging and transparency are effortless.

### Open by Default
Core tooling (Edgit, Conductor) is open source. Cloud is proprietary—we charge for the UI and managed service, not the runtime.

---

## Status

**Edgit:** v0.2.0 (active development)
- ✅ Component versioning
- ✅ Deployment management
- ✅ AI-powered commits
- ✅ Smart detection
- ✅ Component listing (tree, JSON, YAML, table)
- ✅ Discovery tools (scan, detect, patterns)

**Conductor:** v1.5.0 (production-ready)
- ✅ Core runtime with graph executor
- ✅ State management (immutable, access tracking)
- ✅ Agent operations: think, code, storage, http, tools, scoring, email, sms, form, page, html, pdf
- ✅ Durable Objects (ExecutionState, HITL)
- ✅ Webhooks & scheduled execution
- ✅ Built-in testing (741 tests passing)
- ✅ CLI tools (exec, agents, test, docs, history, logs, state, replay, health, config)
- ✅ SDK with client library & testing utilities
- ✅ Observability & logging
- ✅ AI Gateway integration
- ✅ Scoring system for quality metrics
- 📋 MCP integration (tools operation ready)

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

### Install Conductor
```bash
# Install Conductor
npm install @ensemble-edge/conductor

# Create ensemble workflow
mkdir -p ensembles
cat > ensembles/hello-world.yml << EOF
name: hello-world
flow:
  - agent: greet
    operation: think
    config:
      provider: cloudflare
      model: '@cf/meta/llama-3.1-8b-instruct'
    input:
      prompt: Say hello to \${input.name}
EOF

# Deploy to Cloudflare Workers
npx wrangler login
npx wrangler deploy

# Execute ensemble
curl https://your-worker.workers.dev/api/v1/execute \
  -H "Content-Type: application/json" \
  -d '{"ensemble": "hello-world", "input": {"name": "World"}}'
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

- **[Edgit](https://docs.ensemble.ai/edgit/overview)** - Component versioning system
- **[Conductor](https://docs.ensemble.ai/conductor/overview)** - Edge orchestration framework
- **[Cloud](https://docs.ensemble.ai/cloud/overview)** - Managed service (coming soon)
- **[Getting Started](https://docs.ensemble.ai/getting-started)** - Quick start guides for all products
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
