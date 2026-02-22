[English](README.md) | [繁體中文](README.zh.md)

# company-intel

Investigate a company's background using public data for pre-negotiation due diligence.

## Description

Company Intel scrapes public registries and databases to extract company registration data, financial signals, and contact information — optimized for Taiwan-based businesses.

## Features

- Fetches company registration data from public registries
- Assesses financial capacity indicators from available signals
- Generates a structured intelligence report in Markdown
- Optimized for Taiwan-based companies (company number lookup)
- Aggregates data from multiple public sources in one call
- Helps you enter negotiations with verified background knowledge

## Usage

Invoke by asking Claude Code with trigger phrases such as:

- "investigate this company"
- "background check"
- "due diligence"
- "調查這家公司"
- "客戶背景調查"

## Related Skills

- [`competitive-intel`](https://github.com/joneshong-skills/competitive-intel)
- [`quote-consultant`](https://github.com/joneshong-skills/quote-consultant)

## Install

Copy the skill directory into your Claude Code skills folder:

```
cp -r company-intel ~/.claude/skills/
```

Skills placed in `~/.claude/skills/` are auto-discovered by Claude Code. No additional registration is needed.
