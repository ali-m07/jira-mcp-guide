# راهنمای Jira MCP 🛠️

آموزش فارسیِ اتصال Jira به دستیارهای هوش مصنوعی (Claude Code، Claude Desktop، Cursor و …) با استفاده از پروتکل **MCP** — بدون نیاز به نوشتن هیچ کدی.

> MCP چیست؟ [Model Context Protocol](https://modelcontextprotocol.io) یک استاندارد باز است که به مدل‌های زبانی اجازه می‌دهد به ابزارهای خارجی — مثل Jira — وصل شوند.

---

## این ریپو چیست؟

اینجا هیچ کدی توسعه داده نمی‌شود. این یک **راهنمای کاربردی** است برای اینکه با چند دستور ساده، دستیار هوش مصنوعی‌ات را به Jira وصل کنی تا بتوانی:

- 🔍 تیکت‌ها را با زبان طبیعی جستجو کنی (به‌جای JQL دستی)
- 📋 خلاصه‌ی یک تیکت و کامنت‌هایش را بگیری
- 📊 وضعیت اسپرینت یا پروژه را بپرسی
- ✍️ توضیح تیکت را بازنویسی کنی یا باگ‌ریپورت بنویسی

ابزار اصلی که معرفی می‌کنیم پروژه‌ی متن‌باز [sooperset/mcp-atlassian](https://github.com/sooperset/mcp-atlassian) است که Jira و Confluence را پوشش می‌دهد.

---

## شروع سریع

### پیش‌نیازها

- Python 3.10+ یا Docker (برای نسخه‌ی لوکال)
- یک اکانت Jira (Cloud یا Data Center/Server)
- دستیار هوش مصنوعی که MCP را پشتیبانی کند

### گزینه ۱ — Jira Cloud با API Token

1. از [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens) یک API Token بساز.
2. کانکتور را اضافه کن:

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://your-company.atlassian.net \
  --env JIRA_USERNAME=you@company.com \
  --env JIRA_API_TOKEN=your-token \
  -- uvx mcp-atlassian
```

### گزینه ۲ — Jira Data Center / Server با PAT

در Jira: Profile → **Personal Access Tokens** → توکن بساز، بعد:

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://jira.your-company.com \
  --env JIRA_PERSONAL_ACCESS_TOKEN=your-pat \
  -- uvx mcp-atlassian
```

### گزینه ۳ — سرور ریموت (HTTP)

اگر تیم‌تان یک سرور mcp-atlassian مستقر کرده، فقط آدرسش را بده:

```bash
claude mcp add --scope user --transport http jira https://mcp.your-company.com/mcp
```

### بررسی اتصال

```bash
claude mcp list
# jira: ... - ✓ Connected
```

داخل دستیار، دستور `/mcp` را بزن تا لیست ابزارها را ببینی.

> 📖 راهنمای کامل‌تر برای Claude Desktop و Cursor: [docs/clients.md](docs/clients.md)

---

## نمونه‌کارها

بعد از اتصال، این‌ها را امتحان کن:

| به زبان طبیعی | پشت صحنه |
|---|---|
| «تیکت‌های باز من را پیدا کن» | `jira_search` با JQL: `assignee = currentUser() AND status != Done` |
| «خلاصه‌ی تیکت PROJ-1234 و کامنت‌هایش را بده» | `jira_get_issue` + `jira_get_comments` |
| «باگ‌های اسپرینت جاری را اولویت‌بندی کن» | `jira_search` + تحلیل توسط مدل |
| «برای این باگ یک تیکت با استاندارد پروژه‌مان بنویس» | `jira_create_issue` (در حالت read/write) |

نمونه‌های JQL بیشتر: [docs/jql-cheatsheet.md](docs/jql-cheatsheet.md)

---

## امنیت

- توکن‌ها فقط لوکال می‌مانند (متغیر محیطی یا کش OAuth در `~/.mcp-auth/`).
- هر درخواست با دسترسی‌های خودِ اکانت تو اجرا می‌شود — چیزی بیشتر از دسترسی خودت نمی‌بینی.
- اگر فقط خواندن کافی است، سرور را در حالت `READ_ONLY` اجرا کن.

---

## منابع

- [sooperset/mcp-atlassian](https://github.com/sooperset/mcp-atlassian) — سرور متن‌باز Jira/Confluence MCP
- [Model Context Protocol](https://modelcontextprotocol.io) — مستندات رسمی MCP
- [Atlassian REST API](https://developer.atlassian.com/cloud/jira/platform/rest/v3/) — مرجع API

---

## لایسنس

MIT — آزاد در استفاده و بازنشر.
