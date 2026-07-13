# Task 9: Exploiting SQL Injection in DVWA

## Objective

The objective of this project is to demonstrate how a SQL Injection (SQLi) vulnerability can be identified and exploited in a controlled lab environment using Damn Vulnerable Web Application (DVWA). This exercise is performed solely for educational purposes on an intentionally vulnerable application to understand the risks associated with insecure database queries and to learn effective mitigation techniques.

---

## Lab Environment

* **Operating System:** Kali Linux
* **Target Application:** Damn Vulnerable Web Application (DVWA)
* **Web Server:** Apache2
* **Database:** MariaDB/MySQL
* **Security Level:** Low
* **Target URL:** `http://127.0.0.1/dvwa`

---

## Tools Used

* Kali Linux
* DVWA
* Apache2
* MariaDB
* Web Browser (Firefox/Chrome)

---

## Procedure

### 1. Set Up DVWA

* Started the Apache and MariaDB services.
* Configured DVWA.
* Logged in using the default administrator credentials.
* Set the DVWA security level to **Low**.

---

### 2. Tested Normal Input

The SQL Injection module was accessed and a valid user ID was entered to verify that the application was functioning correctly.

Example:

```text
1
```

The application returned the expected user information.

---

### 3. Performed SQL Injection

A SQL Injection payload was submitted through the vulnerable input field to manipulate the SQL query executed by the application.

Example payload:

```sql
1' OR '1'='1
```

The application returned multiple records instead of a single user, demonstrating that user input was being interpreted as part of the SQL query.

---

### 4. Database Enumeration

Additional SQL Injection techniques were used to:

* Determine the number of columns
* Identify the current database
* Enumerate database tables
* Enumerate table columns
* Display usernames and password hashes stored in the application database

This demonstrates how attackers can extract sensitive information when applications do not properly validate user input.

---

## Why the Vulnerability Exists

SQL Injection occurs because the application directly inserts user-supplied input into SQL statements without proper validation or parameterization.

For example, the application may construct a query similar to:

```sql
SELECT first_name, last_name
FROM users
WHERE user_id = '$id';
```

When the user enters a malicious payload such as:

```sql
1' OR '1'='1
```

the resulting query becomes:

```sql
SELECT first_name, last_name
FROM users
WHERE user_id='1' OR '1'='1';
```

Since the condition `'1'='1'` always evaluates to **TRUE**, the database returns all matching records rather than only the intended result.

This vulnerability exists because user input is treated as executable SQL code instead of being handled strictly as data.

---

## Security Impact

A successful SQL Injection attack may allow an attacker to:

* Bypass authentication
* Read confidential database information
* Enumerate databases, tables, and columns
* Retrieve usernames and password hashes
* Modify existing records
* Delete critical data
* Execute administrative database commands (depending on database permissions)

SQL Injection remains one of the most serious web application vulnerabilities because it directly compromises the application's backend database.

---

## Mitigation Strategies

Developers can significantly reduce the risk of SQL Injection by implementing the following security measures:

* Use **parameterized queries (prepared statements)** for all database interactions.
* Never concatenate user input directly into SQL queries.
* Validate and sanitize all user input.
* Apply server-side input validation rather than relying only on client-side checks.
* Use the principle of least privilege for database accounts.
* Display generic error messages instead of exposing database errors.
* Keep application frameworks, libraries, and database servers up to date.
* Perform regular code reviews and penetration testing.
* Deploy a Web Application Firewall (WAF) to detect and block malicious requests.

---

## Importance of Parameterized Queries

Parameterized queries (also known as prepared statements) separate SQL commands from user-supplied data. Instead of interpreting user input as executable SQL, the database treats it strictly as data values.

For example, instead of constructing SQL statements by concatenating strings, applications should use prepared statements with placeholders for user input. This prevents malicious input from changing the structure or logic of the SQL query.

Because parameterized queries ensure that user input cannot alter the intended SQL command, they are considered the most effective defense against SQL Injection and are recommended by the OWASP Top 10 secure coding guidelines.

---

## Conclusion

This project demonstrated how a SQL Injection vulnerability can be exploited in an intentionally vulnerable web application (DVWA). The exercise highlighted how insecure handling of user input can expose sensitive information and compromise an application's database.

The lab also emphasized the importance of secure coding practices. By using parameterized queries, validating user input, following the principle of least privilege, and performing regular security testing, developers can effectively protect web applications against SQL Injection attacks.

This demonstration was conducted in a controlled laboratory environment for educational purposes only. SQL Injection techniques should never be used against systems without explicit authorization.

---

## Repository Structure

```text
Task-9-SQL-Injection/
│
├── README.md
├── sql_injection_exploit.sh
└── sql.mp4
```

---

## Disclaimer

This project was performed only in a local lab environment using DVWA, an intentionally vulnerable web application designed for cybersecurity education. The techniques demonstrated are intended solely for learning and authorized security testing. Unauthorized testing of systems without permission is illegal and unethical.
