# اتصال به کلاینت‌های مختلف

راهنمای تنظیم Jira MCP در کلاینت‌های رایج. مقادیر `<...>` را با اطلاعات خودت جایگزین کن.

---

## Claude Code (CLI)

### OAuth / API Token (Jira Cloud)

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://your-company.atlassian.net \
  --env JIRA_USERNAME=you@company.com \
  --env JIRA_API_TOKEN=<API_TOKEN> \
  -- uvx mcp-atlassian
```

`--scope user` یعنی کانکتور در همه‌ی پروژه‌ها در دسترس است؛ حذفش کن تا فقط برای پروژه‌ی فعلی باشد.

### Personal Access Token (Data Center / Server)

```bash
claude mcp add --scope user jira \
  --env JIRA_URL=https://jira.your-company.com \
  --env JIRA_PERSONAL_ACCESS_TOKEN=<PAT> \
  -- uvx mcp-atlassian
```

### مدیریت

```bash
claude mcp list              # وضعیت اتصال
claude mcp remove jira -s user
```

برای احراز هویت دوباره: `/mcp` → انتخاب کانکتور → **Authenticate**.

---

## Claude Desktop

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

اگر `uvx` نداری:

```bash
# macOS
brew install uv
# ویندوز / لینوکس
pip install uv
```

---

## Cursor

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

---

## VS Code (Copilot Chat)

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

---

## عیب‌یابی

| مشکل | علت | راه‌حل |
|---|---|---|
| `✗ Failed to connect` | احراز هویت نشده / توکن نامعتبر | توکن را دوباره بساز؛ کش را پاک کن: `rm -rf ~/.mcp-auth/` |
| `uvx: command not found` | uv نصب نیست | `pip install uv` یا `brew install uv` |
| ابزارها بعد از اتصال نیستند | اکانت اشتباه / بدون دسترسی | با همان اکانت دستی وارد Jira شو و دسترسی را چک کن |
| خطای 401 با PAT | توکن منقضی شده | PAT جدید بساز و کانفیگ را به‌روز کن |
| نتیجه‌ی خالی در جستجو | دسترسی محدود به پروژه | با JQL روی پروژه‌ای که دسترسی داری امتحان کن |
