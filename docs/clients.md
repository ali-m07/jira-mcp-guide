<div align="center">

# 🔌 اتصال به کلاینت‌ها · Client Setup

**تنظیم Jira MCP در کلاینت‌های رایج · Setting up Jira MCP in common clients**

[🇮🇷 فارسی](#-فارسی) · [🇬🇧 English](#-english)

[← بازگشت به README / Back to README](../README.md)

</div>

---

## 🇮🇷 فارسی

مقادیر `<...>` را با اطلاعات خودت جایگزین کن.

### [CC] (CLI)

**OAuth / API Token (Jira Cloud):**

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://your-company.atlassian.net \
  --env JIRA_USERNAME=you@company.com \
  --env JIRA_API_TOKEN=<API_TOKEN> \
  -- uvx mcp-atlassian
```

**Personal Access Token (Data Center / Server):**

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://jira.your-company.com \
  --env JIRA_PERSONAL_ACCESS_TOKEN=<PAT> \
  -- uvx mcp-atlassian
```

> `--scope user` یعنی کانکتور در همه‌ی پروژه‌ها در دسترس است؛ حذفش کن تا فقط برای پروژه‌ی فعلی باشد.

**مدیریت:**

```bash
claude mcp list              # وضعیت اتصال
claude mcp remove jira -s user
```

برای احراز هویت دوباره: `/mcp` → انتخاب کانکتور → **Authenticate**.

### Claude Desktop

پیش‌نیاز: نصب بودن Node.js (`node --version`).

فایل کانفیگ را باز کن (Settings → Developer → Edit Config) و اضافه کن:

```json
{
  "mcpServers": {
    "jira": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_USERNAME": "you@company.com",
        "JIRA_API_TOKEN": "<API_TOKEN>"
      }
    }
  }
}
```

ذخیره کن و Claude Desktop را کامل ببند و دوباره باز کن. آیکن چکش پایین چت یعنی ابزارها آماده‌اند.

اگر `uvx` نداری: `pip install uv` یا `brew install uv`.

### Cursor

فایل `.cursor/mcp.json` (برای پروژه) یا `~/.cursor/mcp.json` (گلوبال):

```json
{
  "mcpServers": {
    "jira": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_USERNAME": "you@company.com",
        "JIRA_API_TOKEN": "<API_TOKEN>"
      }
    }
  }
}
```

سپس در تنظیمات Cursor → **MCP**، سرور را Enable کن.

### VS Code (Copilot Chat)

در `settings.json`:

```json
{
  "mcp": {
    "servers": {
      "jira": {
        "command": "uvx",
        "args": ["mcp-atlassian"],
        "env": {
          "JIRA_URL": "https://your-company.atlassian.net",
          "JIRA_USERNAME": "you@company.com",
          "JIRA_API_TOKEN": "<API_TOKEN>"
        }
      }
    }
  }
}
```

### 🧯 عیب‌یابی

| مشکل | علت | راه‌حل |
|---|---|---|
| `✗ Failed to connect` | احراز هویت نشده / توکن نامعتبر | توکن را دوباره بساز؛ کش را پاک کن: `rm -rf ~/.mcp-auth/` |
| `uvx: command not found` | uv نصب نیست | `pip install uv` یا `brew install uv` |
| ابزارها بعد از اتصال نیستند | اکانت اشتباه / بدون دسترسی | با همان اکانت دستی وارد Jira شو و دسترسی را چک کن |
| خطای 401 با PAT | توکن منقضی شده | PAT جدید بساز و کانفیگ را به‌روز کن |
| نتیجه‌ی خالی در جستجو | دسترسی محدود به پروژه | با JQL روی پروژه‌ای که دسترسی داری امتحان کن |

---

## 🇬🇧 English

Replace `<...>` with your own values.

### [CC] (CLI)

**OAuth / API Token (Jira Cloud):**

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://your-company.atlassian.net \
  --env JIRA_USERNAME=you@company.com \
  --env JIRA_API_TOKEN=<API_TOKEN> \
  -- uvx mcp-atlassian
```

**Personal Access Token (Data Center / Server):**

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://jira.your-company.com \
  --env JIRA_PERSONAL_ACCESS_TOKEN=<PAT> \
  -- uvx mcp-atlassian
```

> `--scope user` makes the connector available in all your projects; drop it to scope it to the current project only.

**Management:**

```bash
claude mcp list              # connection status
claude mcp remove jira -s user
```

To re-authenticate: `/mcp` → pick the connector → **Authenticate**.

### Claude Desktop

Prerequisite: Node.js installed (`node --version`).

Open the config file (Settings → Developer → Edit Config) and add:

```json
{
  "mcpServers": {
    "jira": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_USERNAME": "you@company.com",
        "JIRA_API_TOKEN": "<API_TOKEN>"
      }
    }
  }
}
```

Save, then fully quit and reopen Claude Desktop. A hammer icon at the bottom of the chat means the tools are ready.

Missing `uvx`? Install it with `pip install uv` or `brew install uv`.

### Cursor

In `.cursor/mcp.json` (per project) or `~/.cursor/mcp.json` (global):

```json
{
  "mcpServers": {
    "jira": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_USERNAME": "you@company.com",
        "JIRA_API_TOKEN": "<API_TOKEN>"
      }
    }
  }
}
```

Then enable the server under Cursor settings → **MCP**.

### VS Code (Copilot Chat)

In `settings.json`:

```json
{
  "mcp": {
    "servers": {
      "jira": {
        "command": "uvx",
        "args": ["mcp-atlassian"],
        "env": {
          "JIRA_URL": "https://your-company.atlassian.net",
          "JIRA_USERNAME": "you@company.com",
          "JIRA_API_TOKEN": "<API_TOKEN>"
        }
      }
    }
  }
}
```

### 🧯 Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| `✗ Failed to connect` | Not authenticated / invalid token | Regenerate the token; clear the cache: `rm -rf ~/.mcp-auth/` |
| `uvx: command not found` | uv not installed | `pip install uv` or `brew install uv` |
| Tools missing after connect | Wrong account / no access | Log into Jira manually with that account and check access |
| 401 error with a PAT | Token expired | Create a new PAT and update the config |
| Search returns nothing | Project access limited | Try JQL against a project you can access |
