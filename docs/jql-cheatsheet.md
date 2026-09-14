<div align="center">

# 📝 چیت‌شیت JQL · JQL Cheatsheet

**کوئری‌های آماده برای دستیارهای هوش مصنوعی · Ready-made queries for AI assistants**

[🇮🇷 فارسی](#-فارسی) · [🇬🇧 English](#-english)

[← بازگشت به README / Back to README](../README.md)

</div>

---

## 🇮🇷 فارسی

این کوئری‌ها را می‌توانی مستقیم به دستیار بگویی یا در پرامپتت استفاده کنی. دستیار آن‌ها را با ابزار `jira_search` اجرا می‌کند.

### تیکت‌های خودم

```jql
assignee = currentUser() AND status != Done ORDER BY updated DESC
```

```jql
assignee = currentUser() AND created >= -7d
```

### اسپرینت جاری

```jql
project = MYPROJ AND sprint in openSprints() ORDER BY priority DESC
```

```jql
project = MYPROJ AND sprint in openSprints() AND status = "In Progress"
```

### باگ‌ها

```jql
project = MYPROJ AND issuetype = Bug AND priority in (Highest, High) AND status != Done
```

```jql
project = MYPROJ AND issuetype = Bug AND created >= -30d ORDER BY created DESC
```

### بررسی و بازبینی

```jql
project = MYPROJ AND status = "In Review" AND assignee != currentUser()
```

### موارد مانده و عقب‌افتاده

```jql
project = MYPROJ AND duedate < endOfDay() AND status != Done
```

```jql
project = MYPROJ AND updated <= -14d AND status not in (Done, Closed)
```

### اپیک و زیرتسک

```jql
"Epic Link" = MYPROJ-100 AND status != Done
```

### جستجو در کامنت‌ها

```jql
project = MYPROJ AND comment ~ "blocking"
```

### 💡 نکته برای پرامپت‌نویسی

- به جای حفظ کردن JQL، خواسته‌ات را با زبان طبیعی بگو؛ مدل خودش کوئری می‌سازد. اگر نتیجه عجیب بود، ازش بخواه JQL ساخته‌شده را نشان دهد.
- برای پروژه‌های بزرگ، اول به پروژه یا اسپرینت محدود کن تا نتیجه کوتاه و دقیق بماند.
- دستور «فقط شماره تیکت و خلاصه را برگردان» باعث می‌شود خروجی سبک‌تر و خواناتر باشد.

---

## 🇬🇧 English

Feed these queries to your assistant directly or embed them in your prompts — the assistant runs them via the `jira_search` tool.

### My tickets

```jql
assignee = currentUser() AND status != Done ORDER BY updated DESC
```

```jql
assignee = currentUser() AND created >= -7d
```

### Current sprint

```jql
project = MYPROJ AND sprint in openSprints() ORDER BY priority DESC
```

```jql
project = MYPROJ AND sprint in openSprints() AND status = "In Progress"
```

### Bugs

```jql
project = MYPROJ AND issuetype = Bug AND priority in (Highest, High) AND status != Done
```

```jql
project = MYPROJ AND issuetype = Bug AND created >= -30d ORDER BY created DESC
```

### Code review

```jql
project = MYPROJ AND status = "In Review" AND assignee != currentUser()
```

### Overdue and stale items

```jql
project = MYPROJ AND duedate < endOfDay() AND status != Done
```

```jql
project = MYPROJ AND updated <= -14d AND status not in (Done, Closed)
```

### Epics and subtasks

```jql
"Epic Link" = MYPROJ-100 AND status != Done
```

### Comment search

```jql
project = MYPROJ AND comment ~ "blocking"
```

### 💡 Prompting tips

- Don't memorize JQL — state what you want in natural language and let the model build the query. If the results look off, ask it to show the JQL it generated.
- On large projects, scope the query to a project or sprint first so results stay short and precise.
- Asking for "just the ticket key and summary" keeps the output light and readable.
