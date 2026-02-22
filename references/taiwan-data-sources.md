# Taiwan Company Data Sources

Quick-reference for Taiwan-focused company background investigation.

## Primary Sources (Free, Public)

### Business Registration

| Source | URL Pattern | Key Data | Reliability |
|--------|-----------|----------|-------------|
| **台灣公司網 (twincn)** | `twincn.com/{統編}` | 資本額、代表人、登記地址、設立日期、營業項目 | High — mirrors MOEA data |
| **經濟部商工登記** | `findbiz.nat.gov.tw` | Official registration, capital, status | Authoritative (government) |
| **台灣公司資料** | `company.g0v.ronny.tw/id/{統編}` | 歷史變更紀錄、資本額變化、董監事 | High — g0v community project |

### Employment & HR Platforms

| Source | URL Pattern | Key Data | Notes |
|--------|-----------|----------|-------|
| **1111 人力銀行** | `1111.com.tw/corp/{id}` | 員工福利、產業分類、公司簡介、目前職缺數 | Good for employee benefits & culture signals |
| **104 人力銀行** | `104.com.tw/company/{id}` | 員工人數、資本額、產業、公司介紹 | Best for employee count estimates |
| **518 熊班** | `518.com.tw/company-{id}.html` | 薪資行情、職缺、公司評價 | Good for salary benchmarks |

### Industry & Trade Directories

| Source | URL Pattern | Key Data | Notes |
|--------|-----------|----------|-------|
| **CENS.com** | `cens.com/cens/html/en/supplier/supplier_home_{id}.html` | English company profile, product categories, export data | Best for manufacturers/exporters |
| **台灣黃頁 (web66)** | `web66.com.tw/CW{cat}/-B{id}.html` | 產品目錄、詢價紀錄、公司基本資料 | Good for B2B companies |
| **TW17 台灣儀器網** | `tw17.com.tw/allcom_detail.asp?com_ser={id}` | Instrument/equipment companies specifically | Niche but reliable |
| **百索商情網 (BySources)** | `tw.bysources.com/company/show.php?supno={id}` | B2B company profiles, product listings | Good for industrial companies |

### Financial & Credit

| Source | URL Pattern | Key Data | Notes |
|--------|-----------|----------|-------|
| **公開資訊觀測站 (MOPS)** | `mops.twse.com.tw` | Financial statements, revenue, shareholders | **Listed companies only** |
| **信用報告 (CRIF)** | `crif.com.tw` | Credit rating, payment behavior | Paid service |
| **Dun & Bradstreet Taiwan** | `dnb.com.tw` | D-U-N-S number, credit assessment | Paid service |

## Search Strategies

### Step 1: Identify the Company

When given a URL or name, extract:
- **統一編號 (Tax ID)**: 8-digit number, the master key for all lookups
- **Company name variants**: 中文正式名稱 + English name + trade name
- **Representative (代表人)**: For cross-referencing other companies they own

Search queries:
```
"{company name}" 統編
"{company name}" 資本額 公司資料
site:twincn.com "{company name}"
```

### Step 2: Multi-Source Cross-Reference

Always check at least 3 sources. Priority order:
1. `twincn.com/{統編}` — quick structured data
2. `1111.com.tw` or `104.com.tw` — employee count & company description
3. Company's own website — products, clients, self-reported achievements
4. CENS.com / web66 — if B2B or manufacturing

### Step 3: WebFetch Fallback Chain

Some sites block bots or return empty. If WebFetch fails on one source:
```
twincn.com → 1111.com.tw → 104.com.tw → web66.com.tw → CENS.com
```

Use WebSearch as the universal fallback — it aggregates snippets from multiple sources.

## Data Interpretation Guide

### Capital Amount (資本額) Context

| Capital (NTD) | Typical Scale | IT Budget Tolerance |
|--------------|---------------|-------------------|
| < 500 萬 | Micro business, 1-5 people | 10-30 萬 |
| 500 萬 - 2,000 萬 | Small business, 5-30 people | 30-100 萬 |
| 2,000 萬 - 1 億 | Medium business, 30-200 people | 100-500 萬 |
| 1 億 - 10 億 | Large SME | 500 萬 - 2,000 萬 |
| > 10 億 | Enterprise / listed company | 2,000 萬+ |

**Important**: Capital ≠ Revenue. A 1,000 萬 capital company trading high-value equipment
can have annual revenue of 5,000 萬 - 2 億. Capital is a floor indicator, not a ceiling.

### Industry-Specific Budget Signals

| Industry | Budget Tendency | Why |
|----------|----------------|-----|
| Equipment/instrument trading | Medium-High | High margins, B2B, understand ROI |
| Manufacturing | Medium | Tight margins but value efficiency |
| Retail/F&B | Low-Medium | Thin margins, price sensitive |
| Tech/SaaS | High | Understand tech value |
| Professional services (law, accounting) | Medium | Conservative but willing for efficiency |
| Government/education | Medium (but slow) | Budget cycles, procurement rules |

### Years in Business Signals

| Years | Signal |
|-------|--------|
| < 3 | Startup risk, tight budget, but may invest aggressively |
| 3-10 | Established, growing, moderate budgets |
| 10-25 | Stable, proven, reasonable IT budgets |
| 25+ | Legacy business, likely overdue for digital transformation, may have larger budget for "catching up" |

## Common Pitfalls

- **有限公司 (LLC) capital is often minimum**: Many 有限公司 register with minimum capital
  (as low as 1 元 legally). Don't over-index on capital alone.
- **Multiple related companies**: Check if the representative (代表人) owns other companies.
  The group's total capacity may be much larger than one entity suggests.
- **Trade names vs legal names**: A company may operate under a brand name different from
  its registered name. Search both.
- **Website quality ≠ company quality**: Many profitable B2B companies have terrible websites.
  That's often WHY they need your services.
