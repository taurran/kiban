# Cognitive Twin Framework â€” Design Spec
**Date:** 2026-04-26
**Status:** Approved for planning
**Projects:** kiban (open-source) + Kagami (product)
**Author:** taurran + Claude

---

## 1. Overview

### The Problem

Every AI agent today starts from zero. It doesn't know who you are, how you think, or what you'd decide. So it asks. Or it guesses, acts, observes the result, and corrects. Each cycle costs tokens, costs time, and costs your attention. Production agentic loops consume 4xâ€“15x more tokens than standard chat interactions â€” almost entirely due to this iterative narrowing process. The agent isn't doing more work. It's doing the same work repeatedly until it converges on what you would have told it upfront.

Human-in-the-loop confirmation exists not because humans are needed for the decision â€” it exists because the agent doesn't know you well enough to make the decision alone.

### The Solution: Cognitive Twin

A **cognitive twin** is a persistent, structured replica of how a specific person thinks, reasons, values, and decides â€” injected at the **decision policy layer** of the agentic loop. Not the retrieval layer (RAG). Not the output layer (personas). The decision policy layer â€” where the agent decides what to do next.

When a cognitively grounded agent encounters a decision, it doesn't reason from general model priors. It reasons through the twin: what would this person do here, given their documented goals, heuristics, constraints, and patterns? The answer is already in the vault. The agent acts. The loop moves forward.

> **Coined terms.** "Cognitive twin" and "cognitively grounded agent" are first-use in personal AI as product concepts (coined 2026-04-26). Do not substitute with persona-grounded, identity-grounded, or self-grounded â€” these were evaluated and rejected as insufficiently precise.

### The Agentic Loop â€” Standard vs Cognitively Grounded

**Standard agent:**
```
perceive â†’ reason from general priors â†’ act â†’ observe â†’ iterate â†’ iterate â†’ iterate
           â†‘ doesn't know you                    â†‘ correcting toward your intent
```

**Cognitively grounded agent:**
```
perceive â†’ reason through cognitive twin â†’ act â†’ (done, or one correction)
           â†‘ already knows how you decide
```

The twin doesn't eliminate observation â€” it eliminates unnecessary iteration by starting the decision policy at the user's own reasoning, not the model's average guess across all possible humans.

### The Value of Cognitive Grounding

**1. Token efficiency â€” the most immediate, measurable win**

Each iteration of the agentic loop costs 4xâ€“15x the tokens of a direct answer. A cognitively grounded agent that converges in 1â€“2 cycles instead of 6â€“8 reduces operational cost by 60â€“80% on complex agent tasks. At scale â€” hundreds of daily agent actions across projects, tasks, and tools â€” this is the difference between AI that's affordable to run continuously and AI that's too expensive to leave on.

This is not a marginal improvement. It's a structural cost reduction that makes always-on autonomous agents economically viable for individuals.

**2. Human-in-the-loop liberation**

Every human confirmation pause is a context switch â€” you stop what you're doing to approve something an agent should have known. Cognitively grounded agents eliminate confirmation overhead on decisions already encoded in the twin: routing, prioritization, tool selection, communication tone, tradeoff resolution. You're only pulled in for genuine ambiguity â€” new situations the twin hasn't encountered, high-stakes reversible decisions, or explicit flags.

**3. Decision consistency**

Humans are inconsistent. Under stress, fatigue, or distraction, you make different decisions than your considered, documented self would. The cognitive twin holds your *best* reasoning â€” not your 11pm reactive version. A cognitively grounded agent is more consistent than you are in low-stakes routine decisions. This is a feature, not a limitation.

**4. Accumulated reasoning â€” the compounding advantage**

Every decision heuristic, constraint, and preference you document makes the twin more capable. Unlike general model knowledge (static at training cutoff), the cognitive twin compounds with use. Six months of Guide sessions, lint passes, and ingest cycles produces a dramatically more capable agent substrate than day one. The system gets better the longer you use it â€” and that improvement is yours, not shared with any other user.

**5. Portable, model-agnostic autonomy**

The cognitive twin lives in your vault, not in any model's memory. It travels across tools â€” Claude Code, Gemini CLI, Cursor, Kiro, custom agents â€” via `twin-context.md`. A new model, a new tool, a new agent framework reads the same file and immediately has your decision context. You don't re-onboard. You don't re-explain yourself. The twin goes with you.

**6. Auditable decisions**

Because the twin is a structured, readable artifact, agent decisions become auditable. When an agent acts, you can trace exactly which twin entries informed the decision. This is qualitatively different from black-box model behavior â€” the reasoning substrate is visible and correctable. Wrong decision? Update the heuristic. The twin improves.

**7. The trust handoff**

As the twin matures and the override rate drops, the human genuinely becomes optional on a growing category of decisions. This isn't a product claim â€” it's a measurable threshold. When the twin's error rate drops below the human's own inconsistency rate on the same class of decisions, removing the human from that loop is a quality improvement, not a risk. This is the long-term destination: an agent that acts as you would, freeing you for decisions that actually require you.

**8. Coherent agent teams at zero coordination overhead**

Multi-agent orchestration today is expensive and inconsistent because every spawned subagent starts from the same zero â€” no knowledge of who they're working for, no shared decision substrate. The orchestrator spends tokens re-grounding each worker. Workers drift from each other because they're reasoning from general priors, not from a shared source of user intent.

With cognitive twin injection at spawn, every agent in a team is cognitively grounded from the moment it starts. They share the same decision substrate. A worker making a tradeoff between speed and quality doesn't ask the orchestrator â€” it reads your heuristic and acts. The team behaves as if every member knows you personally. Because they do.

```
Standard multi-agent team:
ORCHESTRATOR â†’ re-explains context to Worker A
             â†’ re-explains context to Worker B
             â†’ re-explains context to Worker C
             â†’ arbitrates drift between workers
             â†’ asks human to resolve conflicts

Cognitively grounded team:
ORCHESTRATOR reads twin-context.md once at spawn
             â†’ Worker A inherits twin at reasoning layer
             â†’ Worker B inherits twin at reasoning layer
             â†’ Worker C inherits twin at reasoning layer
             â†’ all three act consistently, no arbitration needed
```

**9. Token efficiency multiplies with team size**

The 60â€“80% per-agent token saving from cognitive grounding doesn't add across a team â€” it multiplies. A 5-agent parallel team that each save 6 iteration cycles represents a 30-cycle total reduction per task. Running complex multi-step workflows with large agent teams becomes economically viable for individuals â€” not just enterprises with infrastructure budgets. This is what makes always-on autonomous personal AI teams a reality rather than a demo.

**10. Persistent team identity across sessions**

Today, spinning up a new agent team means starting over. No memory of prior runs, prior approvals, prior tradeoffs. With the cognitive twin, every new team spawn picks up exactly where the last left off â€” not via conversation history, but because the decision substrate persists in the vault and compounds over time. The team gets better at working for you with every session.

**11. Delegation without full specification**

The hardest part of working with agent teams is writing complete task specifications. You must anticipate every decision the agent might face and document how to handle it â€” or it asks, guesses wrong, or halts. A cognitively grounded team reduces specification burden dramatically. You describe *what* to do. The twin already encodes *how you'd want it done*. At high twin fidelity, the gap between intent and execution approaches zero. You delegate the outcome, not the method.

---

## 2. Product Architecture

### Two-Layer System

```
KIBAN (open-source)
â”œâ”€â”€ Ember agent          â€” structured intake, initializes cognitive twin
â”œâ”€â”€ Synthesis layer      â€” WikiLLM pattern, routes by content type
â”œâ”€â”€ Basic lint           â€” coherence pass, gap report
â”œâ”€â”€ TCS (heuristic)      â€” coverage/recency/depth/consistency weighted
â”œâ”€â”€ Twin output layer    â€” twin-context.md, twin.json, MCP tools
â””â”€â”€ Agent reasoning      â€” identity-grounded actions, gated by TCS

KAGAMI (paid product)
â”œâ”€â”€ Socratic Guide       â€” deepens twin through conversation over time
â”œâ”€â”€ Advanced lint        â€” temporal decay, belief drift, Guide agenda feed
â”œâ”€â”€ TCS (mature)         â€” adds validation/override signal, 91-100% ceiling
â”œâ”€â”€ Insight Engine       â€” pattern recognition, hidden connections
â””â”€â”€ Mature agent autonomy â€” full cognitive twin decision replacement
```

### The OSS/Paid Boundary Is Structural, Not Artificial

The signals that push TCS above ~88% only exist after Socratic refinement. The validation signal (override rate history) and temporal decay model require the Guide to build up. The kiban formula cannot produce 91%+ TCS. You earn the top tier through Kagami. This survives forking â€” there is no arbitrary feature gate to remove.

### The Funnel Mechanic

Ember (free) â†’ structured intake â†’ gap report â†’ one scripted Guide-taste exchange â†’ hard stop with "continue in Kagami." The gap report is designed to create an itch â€” it shows you all gaps but won't close them. One exchange shows what closing a gap feels like. That combination converts.

---

## 3. The Synthesis Pipeline

### Guiding Principle: Route by Content Type

WikiLLM treats all content as a homogeneous corpus. Personal knowledge is not homogeneous. A belief statement and a project update require completely different synthesis strategies. The classifier routes before synthesis.

### Input Sources

```
INPUT SOURCES
â”œâ”€â”€ Direct (Ember intake, text paste, file drop, URL)
â””â”€â”€ MCP Sources (kiban â€” flexible, user-configured)
    â”œâ”€â”€ Obsidian MCP        â€” vault read/write
    â”œâ”€â”€ GitHub MCP          â€” issues, PRs, commit context
    â”œâ”€â”€ Calendar MCP        â€” events, time context
    â”œâ”€â”€ Slack/Discord MCP   â€” conversation capture
    â”œâ”€â”€ Browser MCP         â€” page capture, reading list
    â”œâ”€â”€ Custom MCP          â€” anything user wires up
    â””â”€â”€ Community connectors â€” extensible via SPEC.md contract
```

### MCP Normalization Layer

Every MCP source produces differently-shaped data. The normalization step wraps all inputs in a standard ingest envelope before synthesis:

```json
{
  "source": "github-mcp",
  "source_id": "issue-#142",
  "captured_at": "2026-04-26T14:32:00Z",
  "content_type": "hint:task",
  "raw": "...",
  "metadata": { "repo": "kiban", "labels": ["enhancement"] }
}
```

`content_type` is a soft hint â€” the classifier makes the final routing decision.

### Safety-Net Pattern (from OB1)

Raw input is **always** written to `_sources/` unconditionally before any routing or synthesis. Content is never lost regardless of classification outcome.

### Schema-Aware Multi-Write (from OB1, extended)

One input can simultaneously update multiple destinations. The LLM extracts all entity types in one pass:

```
CLASSIFY + MULTI-WRITE
â”œâ”€â”€ Self-knowledge (beliefs, values, goals, patterns)  â†’ 10-SELF / TELOS
â”œâ”€â”€ Daily / temporal (journal entries, reflections)    â†’ 20-JOURNAL
â”œâ”€â”€ Reference knowledge (projects, tools, concepts)    â†’ 50-KNOWLEDGE wiki
â”œâ”€â”€ Tasks / actions (todos, decisions, commitments)    â†’ 30-PROJECTS
â”œâ”€â”€ People entities (names resolved via fuzzy match)   â†’ people registry
â””â”€â”€ Action items (first-person intent only)            â†’ action item store
```

The LLM prompt is the single source of truth for routing â€” modifying the extraction instructions changes classification behavior.

### Synthesis Operations

| Operation | Trigger | Kiban | Kagami |
|---|---|---|---|
| Ingest | New source arrives | âœ“ full | âœ“ richer |
| Query-write | User queries vault | âœ“ basic | âœ“ full |
| Merge | Duplicate detected (fingerprint) | âœ“ | âœ“ |
| Backlink | Entity mentioned | âœ“ | âœ“ |
| Lint | Scheduled or on-demand | âœ“ basic | âœ“ advanced |
| Decay flag | Belief/goal timestamped + flagged | âœ— | âœ“ |
| Drift detect | Same topic across time | âœ— | âœ“ |

### Vault Log Format

`log.md` is append-only. Kagami's temporal decay model reads it.

```markdown
## 2026-04-26 14:32 | INGEST
Source: "notes on API design tradeoffs.md"
Classified as: reference knowledge
Pages touched: 4
New pages created: 1 (rate-limiting-patterns.md)
Lint queued: yes

## 2026-04-26 14:33 | LINT
Contradictions found: 1
Orphans found: 2
Coverage gaps: heuristics domain at 28%
TCS impact: consistency -2pts, coverage unchanged
```

---

## 4. Lint Architecture

### Kiban Basic Lint â€” coherence pass

Makes the wiki **coherent**.

```
â”œâ”€â”€ Contradiction scan      â€” two pages assert opposing facts â†’ flags both, shows side-by-side
â”œâ”€â”€ Orphan detection        â€” no inbound + no outbound links â†’ flags for merge or delete
â”œâ”€â”€ Stale surface scan      â€” not touched in > N days â†’ flags, does NOT auto-update
â”œâ”€â”€ Duplicate fingerprint   â€” SHA-256 content hash similarity â†’ flags for merge review
â””â”€â”€ Coverage gap            â€” TELOS domains with no content â†’ feeds TCS coverage signal
```

Output: `LINT-REPORT.md` â€” human-readable, machine-parseable, feeds TCS directly.

### Kagami Advanced Lint â€” makes the twin trustworthy

Inherits all kiban checks, adds:

```
â”œâ”€â”€ Temporal decay          â€” beliefs/goals have timestamps; stale high-confidence = flag
â”‚                             Decay rate differs by domain (goals > values)
â”‚                             Stale high-confidence claim is WORSE than no claim
â”‚
â”œâ”€â”€ Belief drift detection  â€” same topic across journal entries over time
â”‚                             Detects evolution, writes drift summary as TELOS page
â”‚                             Feeds Guide: "You've changed position on X three times"
â”‚
â”œâ”€â”€ Guide agenda feed       â€” lint output becomes ordered question queue for Guide sessions
â”‚                             Highest-impact gaps surfaced first
â”‚                             Questions specific to YOUR vault, not generic prompts
â”‚
â””â”€â”€ Twin fidelity audit     â€” maps lint findings to TCS signals
                              Computes per-domain confidence degradation
                              Surfaces: "Your heuristics domain decayed 12pts since last session"
```

---

## 5. Twin Confidence Score (TCS)

### Formula

```
TCS = (Coverage Ã— W1) + (Recency Ã— W2) + (Depth Ã— W3)
    + (Consistency Ã— W4) + (Validation Ã— W5)
```

### Signal Definitions

| Signal | What it measures | Formula |
|---|---|---|
| Coverage | % of 5 domains with â‰¥1 substantive entry | domains_filled / 5 |
| Recency | Weighted average freshness per domain | 100% at 7 days â†’ 0% at 90 days |
| Depth | Log-normalized entry count per domain | log scale prevents thin-entry gaming |
| Consistency | Absence of contradictions | 1 - (contradictions / total_claims) |
| Validation | Agent override rate (last 30 actions) | starts neutral at 0.5 until 10+ actions |

### Weights by Platform

| Signal | Kiban | Kagami |
|---|---|---|
| Coverage | 35% | 15% |
| Recency | 25% | 20% |
| Depth | 20% | 15% |
| Consistency | 15% | 20% |
| Validation | 5% | 30% |

Kiban weights content presence. Kagami weights accuracy. Kagami's formula cannot be reverse-engineered from kiban alone â€” the validation signal requires Guide session history to build up meaningfully.

### TCS Gates (kiban agent behavior)

| Range | Label | Agent Behavior |
|---|---|---|
| 0â€“30% | Initializing | Informational only â€” synthesizes, suggests, never acts |
| 31â€“55% | Emerging | Low-stakes actions (routing, tagging, synthesis) |
| 56â€“75% | Grounded | Project-level decisions, confirm on external actions |
| 76â€“88% | Reliable | Full kiban agent autonomy |
| 89â€“100% | Mature | Kagami-exclusive â€” requires Guide refinement + validation history |

**kiban TCS ceiling: ~88% by formula.** The validation signal contributes only 5% weight in kiban and starts neutral (0.5) without override history. The temporal decay model required to push consistency above ~92% doesn't exist in kiban. These are formula constraints, not feature gates â€” forking kiban cannot bypass them.

### TCS Dashboard (kiban UX)

```
Cognitive Twin: 48% â€” Emerging

  Projects     â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–‘â–‘  78%
  Goals        â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–‘â–‘â–‘â–‘  61%
  Heuristics   â–ˆâ–ˆâ–ˆâ–‘â–‘â–‘â–‘â–‘â–‘â–‘  28%   â† weakest link
  Constraints  â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–‘â–‘â–‘â–‘â–‘  49%
  Tools        â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–‘â–‘  73%

  Agent actions available: note routing, synthesis, tagging
  Locked until 56%: project-level decisions

  â†’ Add 3 heuristics to reach Grounded
```

### Trust Progression Model

| Stage | Twin Fidelity | Human Role |
|---|---|---|
| Initialization (Ember) | Low â€” skeleton | Approves all agent actions |
| Early Kagami | Medium â€” patterns emerging | Approves high-stakes actions |
| Mature Kagami | High â€” consistent, linted, refined | Reviews outputs, rarely blocks |
| Threshold | Twin error rate < human inconsistency rate | Human-in-loop becomes optional |

**The threshold** is measurable: track override rate. When overrides approach zero and remaining overrides are the user changing their mind (not correcting the twin) â€” threshold reached. Not a singularity. A trust handoff.

### Critical Requirement: Temporal Decay

A frozen twin at high fidelity is more dangerous than a low-fidelity one. The twin acts confidently on a past version of you. Beliefs, goals, and priorities must carry timestamps. The lint pass must flag stale high-confidence claims as aggressively as contradictions. This is load-bearing architecture, not a nice-to-have.

---

## 6. Twin Output Layer

The cognitive twin must be consumable by any model or harness that knows where to look. Three contexts:

### 6a. Technical / CLI Users (kiban-native)

```
~/vaults/ai-dev/configs/twin/     â† canonical location (Git-backed, Obsidian Sync)
â”œâ”€â”€ twin.md                       â€” human-readable full twin snapshot
â”œâ”€â”€ twin.json                     â€” machine-readable structured export (versioned)
â”œâ”€â”€ twin-context.md               â€” compressed LLM injection file (~2K tokens)
â”œâ”€â”€ tcs.json                      â€” current TCS + domain breakdown
â””â”€â”€ LINT-REPORT.md                â€” latest lint output
```

Any model reads `twin-context.md` for immediate cognitive twin injection. External harnesses, custom scripts, Claude Code skills all use the same file. kiban's Phase 2 tool adapters @import from this location. The `~/.kiban/twin/` path is a symlink pointing here (same bootstrap pattern as all other kiban tool adapters).

### 6b. GUI Users â€” Obsidian Sync

Twin output files live inside the AI/Dev vault under `configs/twin/`. Obsidian Sync carries them to every device. Any local tool that reads the synced vault path gets access with zero additional configuration.

### 6c. Kagami App â†’ Local Apps

```
KAGAMI OUTPUT SURFACE
â”œâ”€â”€ Local file export       â€” writes twin files to configurable local path
â”œâ”€â”€ MCP tool: query_twin    â€” structured query against twin for agent use
â”œâ”€â”€ REST endpoint           â€” /twin/context, /twin/tcs, /twin/export
â””â”€â”€ Obsidian MCP write      â€” pushes twin updates back to Obsidian vault
```

### The Universal Contract: twin-context.md

Optimized for LLM injection. Every tool reads the same format.

```markdown
# Cognitive Twin â€” [Name]
_Generated: YYYY-MM-DD | TCS: 67% (Grounded) | Version: 1.4_

## Identity
[3-5 sentences: who I am, what I'm building, what drives me]

## Active Projects
[Bulleted, current state + next action for each]

## Goals (30/90 day)
[Prioritized, with deadlines where known]

## Decision Heuristics
[Explicit "I always/never" statements â€” most actionable part for agents]

## Hard Constraints
[Non-negotiables â€” what agents must never do]

## Tool + Workflow Map
[What tools exist, what each is for, how work moves between them]

## Known Gaps (from lint)
[What the twin knows it doesn't know â€” honesty signal for agents]
```

The **Known Gaps** section is critical. It tells consuming agents where not to be confident. An agent reading the twin and seeing "heuristics domain at 28%" knows to ask before acting in that space.

### MCP Tool Surface (kiban)

```
capture       â€” ingest any input, normalize, synthesize
search        â€” semantic search across vault
query_twin    â€” query cognitive twin for a decision context
twin_status   â€” returns TCS, domain breakdown, gap report
lint          â€” trigger lint pass, return report
```

### MCP Tool Surface (Kagami â€” backend-proxied)

Inherits all kiban tools plus:
- Auto-embedding on capture (pgvector, computed at write time not query time)
- Fuzzy entity resolution (three-pass: exact â†’ fuzzy â†’ collision detection)
- Semantic vector search
- Temporal decay queries
- Mobile â†’ FastAPI â†’ MCP proxy architecture (solves localhost problem for mobile)

---

## 7. Ember Agent

### Identity

Ember is the spark. It ignites the cognitive twin. It is efficient and formulaic â€” not Socratic. Ember asks targeted questions and produces artifacts. The contrast with Kagami's Guide is intentional and stark.

### Agent + Skills Architecture

```
kiban/ember/
â”œâ”€â”€ AGENT.md
â”œâ”€â”€ skills/
â”‚   â”œâ”€â”€ intake.md         â€” 5-domain structured interview (formulaic)
â”‚   â”œâ”€â”€ ingest.md         â€” raw input capture + MCP normalization
â”‚   â”œâ”€â”€ synthesize.md     â€” WikiLLM + schema-aware multi-write
â”‚   â”œâ”€â”€ lint.md           â€” basic coherence pass â†’ LINT-REPORT.md
â”‚   â”œâ”€â”€ tcs.md            â€” compute TCS â†’ tcs.json
â”‚   â”œâ”€â”€ export.md         â€” generate twin output files
â”‚   â”œâ”€â”€ gap-report.md     â€” human-readable gap report from lint
â”‚   â””â”€â”€ guide-taste.md    â€” one scripted Guide exchange, Kagami funnel
â””â”€â”€ mcp-sources/
    â”œâ”€â”€ SPEC.md            â€” normalization contract for community connectors
    â”œâ”€â”€ obsidian.md        â€” reference implementation
    â””â”€â”€ github.md          â€” reference implementation
```

### The 5 Intake Domains

Minimum viable twin for day-one agent usefulness:

1. **Active projects** â€” what you're building, current state, next actions
2. **Near-term goals** â€” 30/90-day targets
3. **Decision heuristics** â€” explicit "I always prefer X over Y" (even 3â€“5 unlock significant autonomy)
4. **Hard constraints** â€” non-negotiables, what agents must never do
5. **Tool + workflow map** â€” what tools exist, what each is for, how work moves

### Ember Decision Flow

```
/ember invoked
    â†“
Vault exists? â†’ No: run intake.md
              â†’ Yes: run ingest.md (read existing, find gaps)
    â†“
synthesize.md â†’ lint.md â†’ tcs.md â†’ export.md â†’ gap-report.md
    â†“
TCS â‰¥ 31%? â†’ Yes: run guide-taste.md (one exchange)
           â†’ No: skip (twin too thin for meaningful question)
    â†“
Display TCS dashboard + gap report
"Run /ember ingest to add more context"
"Continue in Kagami â†’ [link]"
```

### Guide-Taste Exchange

After gap report is generated, Ember picks the single highest-impact gap and asks ONE question in Kagami's voice. The user answers. Ember writes the answer to the vault. Hard stop: "Kagami continues this." This is the product demo that runs inside the free tool.

---

## 8. Naming

| Name | Origin | Role |
|---|---|---|
| **kiban** (åŸºç›¤) | Japanese: foundation/infrastructure | The open-source framework |
| **Kagami** (é¡) | Japanese: mirror | The product (paid app) |
| **Ember** | English: the spark that starts the fire | kiban's intake/init agent |
| **Cognitive twin** | Coined 2026-04-26 | The identity-grounded decision substrate |
| **Guide** | Product-facing category term | Kagami's conversation agent type |

Individual Guide personas have their own names and personalities. "Guide" is the category. The persona's name is Hana, or Ryo, etc.

---

## 9. Competitive Positioning

| Competitor | What they do | Our gap |
|---|---|---|
| OB1 / OpenBrain | Memory database + MCP retrieval | No synthesis, no identity grounding, no agent autonomy |
| WikiLLM (Karpathy) | Synthesis at ingest time, compounding wiki | No guided gap-closing, no decision policy layer |
| ChatGPT Memory | Stores what you say | Cloud-locked, passive, no decision grounding |
| Obsidian | PKM tool | No AI synthesis, no agent autonomy |
| RAG systems | Retrieval at query time | Retrieval layer, not decision policy layer |
| Personas | Output style shaping | Wrong layer â€” output, not decision |

**The full loop nobody else has built:**
```
Capture â†’ preserve originals â†’ WikiLLM synthesis â†’ basic lint â†’
gap report â†’ Guide (Socratic, fills gaps) â†’ answers written back â†’
cognitive twin updated â†’ MCP output layer â†’ identity-grounded
autonomous agent actions
```

---

## 10. Kiban Roadmap Impact

This spec adds the following to kiban's existing 5-phase roadmap:

**New phases needed:**
- **Phase 6: Ember Agent** â€” intake skill, ingest + synthesis pipeline, MCP normalization contract, reference connectors (Obsidian, GitHub)
- **Phase 7: Lint + TCS** â€” basic lint pass, TCS computation, twin output layer (twin-context.md, twin.json, MCP tools)
- **Phase 8: Agent Reasoning** â€” TCS-gated agent actions, identity-grounded decision policy injection, kiban-native agent loop

Alternatively: insert as sub-phases of existing roadmap if sequencing permits.

**Kagami roadmap impact** (tracked separately in Kagami project):
- Spec B: Engram Distillation + Agent Integration â€” now has full design spec
- Advanced lint, temporal decay, Guide persona architecture, Kagami MCP proxy layer

---

*Design session: 2026-04-26*
*Referenced: WikiLLM (Karpathy), OB1/OpenBrain (NateBJones-Projects), OpenBrain MCP architecture*
*Status: approved, ready for GSD planning*
