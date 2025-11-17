# CSRF Demonstration Project

A minimal PHP application that demonstrates how a Cross-Site Request Forgery (CSRF) attack works. The project includes user authentication, profile management, and a deliberately vulnerable email update feature, plus a malicious HTML attacker page that exploits the vulnerability.

## Objective

This project shows how CSRF attacks occur and why missing CSRF protections allow attackers to change user data without permission. It includes:

⦁	A working signup and login system

⦁	A vulnerable endpoint (change_email.php)

⦁	An attacker-controlled CSRF exploit page (attack.html)

⦁	A demonstration of the attack workflow

## Features

| Feature                                         | Description       |
|-----------------------------------------------|----------------------------|
| User Login | Basic PHP session-based login using SQLite|
| Signup Page | Register new accounts with username, email, and password|
| Dashboard | Landing page after login|
| Profile Page | View and update email address|
| Vulnerable Email Change | No CSRF token, intentionally insecure|
| CSRF Attack Page | Auto-submits a malicious POST request|

## Technologies Used

- PHP
- SQLITE
- HTML

## How to Run the Project

1.	Start the PHP server:
	php -S localhost:8000 -t public

2. Initialize the database:
	php init_db.php

&emsp;&emsp;This creates:

&emsp;&emsp;&emsp;- database.sqlite

&emsp;&emsp;&emsp;- users table

&emsp;&emsp;You may use the signup page to create a new user.

## CSRF Attack Demonstration
1. Log in as a normal user
	http://localhost:8000/login.php

2. Open the attacker's CSRF page:
	Open attack.html directly from your computer
	The page automatically submits a hidden POST request:

3. Check the result:
	Visit http://localhost:8000/profile.php
	You will see that your email was silently changed to:
		attacker@evil.com
This confirms the CSRF vulnerability.

## Project Structure

```
project/
 ├── public/
 │     ├── login.php
 │     ├── signup.php
 │     ├── dashboard.php
 │     ├── profile.php
 │     ├── change_email.php
 │     ├── logout.php
 |     ├── db.php
 │     └── attack.html
 │
 ├── db/
 │     └── database.sqlite
 │
 |
 └── init_db.php
```
## CSRF Protection (Secure Version)

The /secure_version/ folder contains secure rewritten versions of the application that fix the CSRF vulnerability.

The CSRF fix has 3 parts:

1. CSRF Token Generation (in profile.php)

&emsp;&emsp;A secure random token is generated if it does not exist:

&emsp;&emsp;&emsp;$_SESSION['csrf_token'] = bin2hex(random_bytes(32));

2. Token Added to Form (profile.php)
   
&emsp;&emsp;&emsp;&lt;input type="hidden" name="csrf_token" value="<?= $csrf ?>"&gt;

3. Token Validation (change_email.php)

&emsp;&emsp;&emsp;if ($_POST['csrf_token'] !== $_SESSION['csrf_token']) {
    die("CSRF validation failed. Request blocked.");
}



