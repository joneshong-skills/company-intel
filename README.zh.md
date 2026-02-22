[English](README.md) | [繁體中文](README.zh.md)

# company-intel

Investigate a company's background using public data for pre-negotiation due diligence.

## 說明

Company Intel scrapes public registries and databases to extract company registration data, financial signals, and contact information — optimized for Taiwan-based businesses.

## 功能特色

- Fetches company registration data from public registries
- Assesses financial capacity indicators from available signals
- Generates a structured intelligence report in Markdown
- Optimized for Taiwan-based companies (company number lookup)
- Aggregates data from multiple public sources in one call
- Helps you enter negotiations with verified background knowledge

## 使用方式

透過以下觸發語句呼叫 Claude Code 來使用此技能：

- "investigate this company"
- "background check"
- "due diligence"
- "調查這家公司"
- "客戶背景調查"

## 相關技能

- [`competitive-intel`](https://github.com/joneshong-skills/competitive-intel)
- [`quote-consultant`](https://github.com/joneshong-skills/quote-consultant)

## 安裝

將技能目錄複製到 Claude Code 技能資料夾：

```
cp -r company-intel ~/.claude/skills/
```

放置在 `~/.claude/skills/` 的技能會被 Claude Code 自動發現，無需額外註冊。
