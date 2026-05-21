# SQLi Practice Lab

9 labs, one HTML file, no server. Open it in a browser and start injecting.

Covers INSERT, UPDATE, and DELETE injection — the statement types that get skipped in most practice platforms because everything defaults to SELECT and UNION.

---

## Labs

**INSERT**
- Register as admin by injecting the `role` column the form never shows you
- Second-order: poison your username so a later UPDATE query changes admin's password
- Exfil DB version through a reflected INSERT field

**UPDATE**
- Change another user's password by hijacking the WHERE clause from the password field
- Escalate your role by injecting extra SET columns the app doesn't expose
- Blind time-based injection, char-by-char, through a field with no visible output

**DELETE**
- Wipe every row with `OR 1=1--`
- Stack a `DROP TABLE` after the DELETE using a semicolon
- Use a conditional DELETE as a blind data extraction oracle

---

## Usage

```bash
git clone https://github.com/yourname/sqli-lab
open sqli-lab.html
```

No dependencies. The "database" is a JavaScript object that behaves like MySQL. Live query preview updates as you type so you can see what your payload actually does to the SQL before you fire it.

Two hint levels per lab. DB resets between attempts.

---

## Who it's for

If you're already comfortable with SELECT-based injection and want reps on the stuff that shows up in registration forms, profile pages, and admin panels — this is for that.

Not a beginner resource.

---

## Disclaimer

For education and authorized testing only. Don't test against systems you don't own or have written permission to assess.
