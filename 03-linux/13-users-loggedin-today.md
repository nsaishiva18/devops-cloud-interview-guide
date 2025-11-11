## Question  
How do you get the list of users who logged into the system today?

### 📝 Short Explanation  
This tests your understanding of Linux log inspection and user activity tracking — helpful in auditing, troubleshooting, and system monitoring.

## ✅ Answer  

### 🖥️ Command

```sh
last -F # This will show all users who logged in.
last -F| grep "$(date '+%b %e')"
```

```bash
last | grep "$(date '+%a %b %e')" | awk '{print $1}' | sort | uniq
```

---

### 📘 Detailed Explanation

#### 🔍 Breakdown:
- `last`: Displays recent login history from `/var/log/wtmp`.
- `date '+%a %b %e'`:
  - Outputs today’s date in the format used by `last`, e.g., `Thu Jun 13`.
  - `%a` = abbreviated weekday, `%b` = abbreviated month, `%e` = day of month (with space-padding).
- `grep "$(date ...)"`: Filters only login entries for today.
- `awk '{print $1}'`: Extracts the usernames from the matched lines.
- `sort | uniq`: Removes duplicates to show unique users who logged in today.

---

### ✅ Example Output
```
ubuntu
admin
deploy
```

These are users who successfully logged in on the current date.

---

### 🧠 Bonus Tip
If your system has rotated or missing wtmp logs, this command might show no results. You can verify with:
```bash
ls -lh /var/log/wtmp
```

Or check journal logs:
```bash
journalctl --since today | grep 'session opened'
```

> Summary:  
> This command helps you identify which users logged in today — useful for basic auditing, usage tracking, or verifying automated logins.

---
