<div align="center">

# ⚡ راهنمای Jira MCP · Jira MCP Guide

**آموزش اتصال Jira به دستیارهای هوش مصنوعی با MCP — بدون نوشتن حتی یک خط کد**
**Connect Jira to your AI assistant via MCP — without writing a single line of code**

[🌐 نسخه وب تعاملی / Interactive Web Version](https://ali-m07.github.io/jira-mcp-guide/) · با رابط کاربری و سوییچ زبان · With a UI and a FA/EN toggle

[🇮🇷 فارسی](#-فارسی) · [🇬🇧 English](#-english)

![MCP](https://img.shields.io/badge/protocol-MCP-4c8dff) ![Jira](https://img.shields.io/badge/product-Jira-0052cc) ![No code](https://img.shields.io/badge/custom%20code-none-34d399) ![License](https://img.shields.io/badge/license-MIT-8fa0b8)

</div>

---

## 🇮🇷 فارسی

### این ریپو چیست؟

اینجا هیچ کدی توسعه داده نمی‌شود. این یک **راهنمای کاربردی دوزبانه** است برای اینکه با چند دستور ساده، دستیار هوش مصنوعی‌ات را به Jira وصل کنی تا بتوانی:

- 🔍 تیکت‌ها را با زبان طبیعی جستجو کنی (به‌جای JQL دستی)
- 📋 خلاصه‌ی یک تیکت و کامنت‌هایش را بگیری
- 📊 وضعیت اسپرینت یا پروژه را بپرسی
- ✍️ توضیح تیکت را بازنویسی کنی یا باگ‌ریپورت بنویسی

ابزار اصلی که معرفی می‌کنیم پروژه‌ی متن‌باز [sooperset/mcp-atlassian](https://github.com/sooperset/mcp-atlassian) است که Jira و Confluence را پوشش می‌دهد.

> 💡 **MCP چیست؟** [Model Context Protocol](https://modelcontextprotocol.io) یک استاندارد باز است که به مدل‌های زبانی اجازه می‌دهد به ابزارهای خارجی — مثل Jira — وصل شوند.

### ⚡ شروع سریع

**پیش‌نیازها:** Python 3.10+ یا Docker، یک اکانت Jira، و یک دستیار هوش مصنوعی پشتیبانِ MCP.

<details open>
<summary><b>گزینه ۱ — Jira Cloud با API Token</b></summary>

1. از [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens) یک API Token بساز.
2. کانکتور را اضافه کن:

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://your-company.atlassian.net \
  --env JIRA_USERNAME=you@company.com \
  --env JIRA_API_TOKEN=your-token \
  -- uvx mcp-atlassian
```

</details>

<details>
<summary><b>گزینه ۲ — Jira Data Center / Server با PAT</b></summary>

در Jira: Profile → **Personal Access Tokens** → توکن بساز، بعد:

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://jira.your-company.com \
  --env JIRA_PERSONAL_ACCESS_TOKEN=your-pat \
  -- uvx mcp-atlassian
```

</details>

<details>
<summary><b>گزینه ۳ — سرور ریموت (HTTP)</b></summary>

اگر تیم‌تان یک سرور mcp-atlassian مستقر کرده، فقط آدرسش را بده:

```bash
claude mcp add --scope user --transport http jira https://mcp.your-company.com/mcp
```

</details>

### ✅ بررسی اتصال

```bash
claude mcp list
# jira: ... - ✓ Connected
```

داخل دستیار، دستور `/mcp` را بزن تا لیست ابزارها را ببینی.

### 💬 نمونه‌کارها

بعد از اتصال، با زبان طبیعی حرف بزن — مدل خودش JQL و ابزار مناسب را صدا می‌زند:

| می‌گویی | پشت صحنه |
|---|---|
| «تیکت‌های باز من را پیدا کن» | `jira_search` با `assignee = currentUser() AND status != Done` |
| «خلاصه‌ی تیکت PROJ-1234 و کامنت‌هایش را بده» | `jira_get_issue` + `jira_get_comments` |
| «باگ‌های اسپرینت جاری را اولویت‌بندی کن» | `jira_search` + تحلیل مدل |
| «با استاندارد پروژه یک باگ‌ریپورت بنویس» | `jira_create_issue` (در حالت read/write) |

### 🔒 امنیت

- **توکن لوکال** — توکن‌ها فقط روی سیستم خودت می‌مانند (متغیر محیطی یا کش OAuth در `~/.mcp-auth/`).
- **دسترسی خودت** — هر درخواست با دسترسی‌های اکانت خودت اجرا می‌شود؛ چیزی بیش از دسترسی‌ات نمی‌بینی.
- **حالت فقط‌خواندن** — اگر فقط جستجو و خواندن کافی است، سرور را با `READ_ONLY` اجرا کن.

### 🧯 عیب‌یابی سریع

| مشکل | راه‌حل |
|---|---|
| `✗ Failed to connect` | توکن را دوباره بساز؛ کش را پاک کن: `rm -rf ~/.mcp-auth/` |
| `uvx: command not found` | `pip install uv` یا `brew install uv` |
| خطای 401 با PAT | PAT منقضی شده؛ توکن جدید بساز و کانفیگ را به‌روز کن |
| نتیجه‌ی جستجو خالی است | دسترسی پروژه را چک کن؛ با پروژه‌ای که دسترسی داری امتحان کن |

### 📁 ساختار ریپو

| فایل | توضیح |
|---|---|
| [`docs/clients.md`](docs/clients.md) | تنظیم برای [CC]، Claude Desktop، Cursor و VS Code |
| [`docs/jql-cheatsheet.md`](docs/jql-cheatsheet.md) | کوئری‌های JQL آماده |

### 👤 درباره نویسنده

**علی منصوری** — Solutions Architect با بیش از ۵ سال تجربه در معماری پلتفرم‌های cloud native، اتوماسیون سازمانی و هوش مصنوعی (LLM و RAG). در اسنپ، مالکیت معماری Jira سازمانی و اتوماسیون‌های LLM در حالت production را بر عهده داشته و سامانه‌هایی در خدمت بیش از ۱٬۰۰۰ کاربر داخلی طراحی کرده است. دانشجوی دکتری آینده‌پژوهی (دانشگاه تهران) و کارشناس ارشد MBA.

- 🌐 [LinkedIn](https://linkedin.com/in/ali-mansouri-a7984215b) · [GitHub](https://github.com/ali-m07) · [ایمیل](mailto:ali.mansouri1998@gmail.com)

---

## 🇬🇧 English

### What is this repo?

No code is developed here. This is a **bilingual practical guide** for wiring your AI assistant to Jira with a few simple commands, so you can:

- 🔍 Search tickets in natural language (instead of hand-writing JQL)
- 📋 Get summaries of issues and their comments
- 📊 Ask about sprint or project status
- ✍️ Rewrite ticket descriptions or draft bug reports

The main tool covered is the open-source [sooperset/mcp-atlassian](https://github.com/sooperset/mcp-atlassian) server, which handles both Jira and Confluence.

> 💡 **What is MCP?** The [Model Context Protocol](https://modelcontextprotocol.io) is an open standard that lets language models plug into external tools — like Jira.

### ⚡ Quick Start

**Prerequisites:** Python 3.10+ or Docker, a Jira account, and an MCP-capable AI assistant.

<details open>
<summary><b>Option 1 — Jira Cloud with an API Token</b></summary>

1. Create an API token at [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens).
2. Add the connector:

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://your-company.atlassian.net \
  --env JIRA_USERNAME=you@company.com \
  --env JIRA_API_TOKEN=your-token \
  -- uvx mcp-atlassian
```

</details>

<details>
<summary><b>Option 2 — Jira Data Center / Server with a PAT</b></summary>

In Jira: Profile → **Personal Access Tokens** → create one, then:

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://jira.your-company.com \
  --env JIRA_PERSONAL_ACCESS_TOKEN=your-pat \
  -- uvx mcp-atlassian
```

</details>

<details>
<summary><b>Option 3 — Remote server (HTTP)</b></summary>

If your team already hosts an mcp-atlassian server, just point at it:

```bash
claude mcp add --scope user --transport http jira https://mcp.your-company.com/mcp
```

</details>

### ✅ Verify

```bash
claude mcp list
# jira: ... - ✓ Connected
```

Inside your assistant, run `/mcp` to list the available tools.

### 💬 Examples

Once connected, just talk naturally — the model picks the right tools and builds the JQL for you:

| You ask | Under the hood |
|---|---|
| "Find my open tickets" | `jira_search` with `assignee = currentUser() AND status != Done` |
| "Summarize PROJ-1234 and its comments" | `jira_get_issue` + `jira_get_comments` |
| "Prioritize current sprint bugs" | `jira_search` + model analysis |
| "Write a bug report per our template" | `jira_create_issue` (read/write mode) |

### 🔒 Security

- **Local-only tokens** — tokens stay on your machine (env vars or the OAuth cache in `~/.mcp-auth/`).
- **Your own permissions** — every request runs with your account's access; you never see more than you already can.
- **Read-only mode** — if search-and-read is all you need, run the server with `READ_ONLY`.

### 🧯 Quick troubleshooting

| Problem | Fix |
|---|---|
| `✗ Failed to connect` | Regenerate the token; clear the cache: `rm -rf ~/.mcp-auth/` |
| `uvx: command not found` | `pip install uv` or `brew install uv` |
| 401 error with a PAT | The PAT expired; create a new one and update the config |
| Search returns nothing | Check project permissions; try a project you can access |

### 📁 Repo layout

| File | Description |
|---|---|
| [`docs/clients.md`](docs/clients.md) | Setup for [CC], Claude Desktop, Cursor and VS Code |
| [`docs/jql-cheatsheet.md`](docs/jql-cheatsheet.md) | Ready-made JQL queries |

### 👤 About the Author

**Ali Mansouri** — a Solutions Architect with 5+ years of experience in cloud native platforms, enterprise automation, and AI (LLMs & RAG). At Snapp, he owned the architecture of enterprise Jira and production LLM automation, building systems that serve 1,000+ internal users. PhD candidate in Futures Studies (University of Tehran) with an MBA.

- 🌐 [LinkedIn](https://linkedin.com/in/ali-mansouri-a7984215b) · [GitHub](https://github.com/ali-m07) · [Email](mailto:ali.mansouri1998@gmail.com)

---

<div align="center">

**ساخته‌شده بر پایه‌ی / Built on** [sooperset/mcp-atlassian](https://github.com/sooperset/mcp-atlassian) · [Model Context Protocol](https://modelcontextprotocol.io) · MIT

</div>
