# چیت‌شیت JQL برای دستیارهای هوش مصنوعی

این کوئری‌ها را می‌توانی مستقیم به دستیار بگویی یا در پرامپتت استفاده کنی. دستیار آن‌ها را با ابزار `jira_search` اجرا می‌کند.

## تیکت‌های خودم

```
assignee = currentUser() AND status != Done ORDER BY updated DESC
```

```
assignee = currentUser() AND created >= -7d
```

## اسپرینت جاری

```
project = MYPROJ AND sprint in openSprints() ORDER BY priority DESC
```

```
project = MYPROJ AND sprint in openSprints() AND status = "In Progress"
```

## باگ‌ها

```
project = MYPROJ AND issuetype = Bug AND priority in (Highest, High) AND status != Done
```

```
project = MYPROJ AND issuetype = Bug AND created >= -30d ORDER BY created DESC
```

## بررسی و بازبینی

```
project = MYPROJ AND status = "In Review" AND assignee != currentUser()
```

## موارد مانده و عقب‌افتاده

```
project = MYPROJ AND duedate < endOfDay() AND status != Done
```

```
project = MYPROJ AND updated <= -14d AND status not in (Done, Closed)
```

## اپیک و زیرتسک

```
" Epic Link" = MYPROJ-100 AND status != Done
```

## ترکیب با کامنت‌ها

```
project = MYPROJ AND comment ~ "blocking"
```

---

## نکته برای پرامپت‌نویسی

- به جای حفظ کردن JQL، خواسته‌ات را با زبان طبیعی بگو؛ مدل خودش کوئری می‌سازد. اگر نتیجه عجیب بود، ازش بخواه JQL ساخته‌شده را نشان دهد.
- برای پروژه‌های بزرگ، اول به پروژه یا اسپرینت محدود کن تا نتیجه کوتاه و دقیق بماند.
- دستور «فقط شماره تیکت و خلاصه را برگردان» باعث می‌شود خروجی سبک‌تر و خواناتر باشد.
