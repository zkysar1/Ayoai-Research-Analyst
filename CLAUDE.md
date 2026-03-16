## What This Does

Research documentation repository for AyoAI's autonomous agent behavior systems. Contains organized research notes, academic paper analysis, competitive intelligence, and technical investigation into cognitive architectures, memory systems, and behavioral modeling for intelligent NPCs in game environments (Roblox). This is a knowledge base — no production code, no deployable artifacts.

Safety Tier 4 — Non-Lambda. Test circuit: `syntax-only`. Does not participate in an invocation chain.

## Verification

Documentation-only repository. No code, no build, no tests.

```bash
# Syntax check — verify all markdown files parse cleanly
find . -name "*.md" -not -path "./.git/*" | head -5
```

## Quick Reference

| Field | Value |
|-------|-------|
| Language | Markdown (documentation only) |
| Build | None |
| Tests | None |
| Deploy | None — push to main |
| CI/CD | None |
| Env vars | None |
| Entry point | `Research-Notes/` directory |

## Architecture

```
Research-Notes/
  Core-Concepts/           # Perception, consciousness, personality frameworks
  Implementation/          # ABC method, cognitive architecture, decision augmentation
  Memory-Systems/          # RAG, hierarchical memory, temporal memory
  Planning-and-Reasoning/  # Task decomposition, MCTS, PDDL, utility theory
  Learning-and-Adaptation/ # Reflection, behavior generation
  Market-Research/         # Competitors, monetization, Roblox worlds
  images/                  # media/, media2/, media3/ — sequential image###.png
```

Root-level files:
- `Research Inbox.md` — unprocessed links and ideas queue
- `Listen to.md` — talks and videos to review
- `Official Intelligence Modules.md` — plug-and-play NPC behavior module notes

## Key Files

| File | Purpose |
|------|---------|
| `Research-Notes/Implementation/abc-method.md` | ABC (Antecedent-Behavior-Consequence) behavioral framework |
| `Research-Notes/Implementation/cognitive-architecture.md` | 22-verticle, 97-process cognitive architecture |
| `Research-Notes/Implementation/decision-augmentation.md` | Evidence-based NPC decision making |
| `Research-Notes/Market-Research/competitors.md` | Altera, Inworld, Soul Machines analysis |
| `Research-Notes/Memory-Systems/memory-overview.md` | Memory architecture survey |
| `Research Inbox.md` | Incoming research items to process |

## Integration Points

This research feeds into implementation repositories:
- **Ayoai-Environment-Server** — Java AI engine using these cognitive architectures
- **Ayoai-Public-Web-App** — Dashboard for monitoring NPC behavior
- **Ayoai-Roblox-Integration** — Game integration using ABC frameworks
- **Lambda functions** — Microservices implementing behavioral components

## Constraints

Research-only scope. This repository must NOT:
- Write production code or technical specifications
- Make product/business strategy decisions
- Deploy, manage infrastructure, or handle operations
- Design UI/UX or create user documentation
- Manage API keys, billing, or platform operations

This repository SHOULD:
- Research AI behaviors, cognitive architectures, and agent systems
- Analyze competitors and market trends
- Document findings with Ayoai relevance and implementation ideas
- Support other repos with research backing

Consult Product Manager when research suggests new features or strategic pivots.
Consult Business Analyst when research needs translation to requirements.

### Research Entry Format

```markdown
## Paper/Tool/Concept Name

**Source**: [URL or citation]
**Date Added**: [YYYY-MM-DD]

### Overall Notes
- Key insights
- **Ayoai Relevance**: How this helps autonomous agent behavior

### Details
[Research content]
```

### Naming Conventions
- Files: `specific-topic-name.md` (lowercase, hyphens)
- Directories: `Title-Case-With-Hyphens/`
- Images: `image###.png` (sequential numbering in `Research-Notes/images/media/`)
