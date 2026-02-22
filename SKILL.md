---
name: company-intel
description: >-
  This skill should be used when the user asks to "investigate this company",
  "background check on this client", "research company info", "due diligence",
  "調查這家公司", "客戶背景調查", "公司盡職調查", "查一下這家公司",
  "幫我調查對方", "evaluate client budget capacity", mentions company
  investigation, client research, or discusses assessing a business partner's
  financial capacity before negotiation.
version: 0.2.0
tools: Bash, Read, Write, WebSearch, WebFetch, sandbox_execute
---

# Company Intel

Investigate a company's background using public data sources. Extract registration
data, assess financial capacity, and produce a structured intelligence report for
pre-negotiation or due diligence purposes. Optimized for Taiwan-based companies.

## Agent Delegation

Delegate company research sub-tasks to the `researcher` agent, one per research angle (registration data, supplementary signals, representative lookup), so they run in parallel without saturating main context.

```
Main context (target identification + report synthesis)
  └─ Task(subagent_type: researcher, prompt: "Fetch registration data for {company name / 統編} from twincn.com and 1111.com.tw. Return ONLY structured fields: capital, representative, address, employee count.")
  └─ Task(subagent_type: researcher, prompt: "Search for news, reviews, and reputation signals for {company name}. Return ONLY a bullet-point summary of key findings.")
```

> **Execution routing**: Per-angle research (registration, news, reputation) → delegate to `researcher` agent (uses WebSearch/WebFetch). Cross-source aggregation (3+ sources) → **main context** uses `sandbox_execute` directly (sub-agents cannot access MCP tools).

## Workflow

> **Sandbox limitation**: `sandbox_execute` network is restricted to localhost — it cannot fetch external URLs (twincn, 1111, CENS, etc.). Use `researcher` agents with WebSearch/WebFetch for external data gathering. Sandbox can only aggregate LOCAL files already saved to `~/Claude/` (e.g., cross-referencing pre-fetched results).

### Step 1 — Identify the Target

Collect from the user (ask if missing):

| Required | Info |
|----------|------|
| **Company name or URL** | At least one identifier |
| **Purpose** | Pre-quote, partnership evaluation, due diligence, etc. |

Optional accelerators:
- 統一編號 (Tax ID) — skips search, direct lookup
- Known industry or products
- Geographic market (default: Taiwan)

### Step 2 — Multi-Source Intelligence Gathering

Run searches in parallel where possible.

#### 2a. Name Resolution

Extract company identifiers from the given input:

```
WebSearch: "{company name}" 統編 公司資料
WebSearch: "{company name}" OR "{English name}" 資本額
WebFetch: company website (extract About page, product catalog)
```

If a URL is given, WebFetch the site first to extract the company's legal name,
then search for registration data.

#### 2b. Registration Data

Once the 統編 or company name is confirmed, check structured sources:

1. **twincn.com/{統編}** — capital, representative, address, scope
2. **1111.com.tw** or **104.com.tw** — employee count, benefits, job openings
3. **CENS.com** — English profile, export data (for B2B/manufacturers)

Read `references/taiwan-data-sources.md` for the full source list and fallback chain.

WebFetch each source. If one fails, move to the next in the fallback chain.

#### 2c. Supplementary Signals

Gather contextual intelligence:

```
WebSearch: "{company name}" 評價 OR 口碑 OR 新聞
WebSearch: "{representative name}" 公司 OR 企業
```

Check for:
- News articles (expansion, lawsuits, awards)
- Other companies owned by the same representative
- Industry reputation signals

### Step 3 — Analyze & Assess

Cross-reference all gathered data to assess:

#### Financial Capacity

Read `references/taiwan-data-sources.md` § Data Interpretation Guide for:
- Capital amount context table
- Industry-specific budget signals
- Years in business signals

Formula for IT budget tolerance estimate:
```
Base = Capital amount context table lookup
Adjust for:
  + Industry premium (equipment/tech = higher tolerance)
  + Years in business (25+ years = catching up budget)
  + Multiple entities (group capacity > single entity)
  - Recent cost-cutting signals (no hiring, layoffs)
  - Low-margin industry (retail, F&B)
```

#### Negotiation Intelligence

Estimate:
- **心理底線**: What they'd ideally want to pay (usually 60-70% of fair market)
- **支付上限**: What they'd agree to if ROI is demonstrated (varies by industry)
- **議價傾向**: Based on industry norms (B2B = moderate, retail = aggressive)

### Step 4 — Produce Report

Read `references/report-template.md` for the full template structure.

Determine report depth based on context:

| Context | Depth | Sections |
|---------|-------|----------|
| Quick lookup | Minimal | 1-2 only |
| Pre-negotiation (default) | Standard | 1-6 |
| Large contract / partnership | Full | All sections (1-8) |

Always include confidence levels for inferred data (High/Medium/Low).

## Sandbox Optimization

Batch operations benefit from `sandbox_execute`:

- **Local result aggregation only**: After researchers fetch data via WebSearch/WebFetch, sandbox can cross-reference pre-saved results from `~/Claude/` — NOT for fetching external URLs (sandbox network is localhost only)
- Saves context tokens when merging 3+ pre-fetched result files

Principle: **Deterministic batch work → sandbox; reasoning/presentation → LLM.**

## Integration with Other Skills

- **`/quote-consultant`** — Trigger company-intel as part of Step 1 when a client URL
  or company name is provided. Feed the financial capacity assessment into pricing strategy.
- **`/competitive-intel`** — company-intel provides factual background; competitive-intel
  analyzes market positioning and strategy. Complement, don't overlap.
- **`/smart-search`** — Use for supplementary research when standard data sources
  don't cover the target (e.g., international companies, startups).
- **meeting-insights** — Company context enriches meeting analysis

## Additional Resources

### Reference Files
- **`references/taiwan-data-sources.md`** — Taiwan business databases, URLs, data fields,
  fallback chains, and data interpretation guide (capital context, industry signals)
- **`references/report-template.md`** — Structured report template with sections,
  confidence levels, and depth guidelines
