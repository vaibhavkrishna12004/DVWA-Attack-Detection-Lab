# DVWA-Attack-Detection-Lab
Demonstrates detection of SQL Injection attacks through web server log analysis in DVWA. Includes attack simulation, log extraction, and identification of malicious patterns in a controlled lab environment.
Overview

This project demonstrates how SQL Injection attacks can be detected from a defensive perspective using web server log analysis.

After successfully exploiting a vulnerable DVWA application and extracting sensitive data, the focus shifted toward identifying how such attacks appear in server logs and how they can be detected by defenders.
Attack Context

This project builds upon a previously performed SQL Injection attack on DVWA, where:

- A vulnerable input field was identified
- SQL Injection was exploited using logical and UNION-based payloads
- User credentials were extracted from the database
- Password hashes were cracked using a dictionary attack

Attack Activity

SQL Injection was performed using the following payloads:

1' OR '1'='1
1' UNION SELECT user, password FROM users --
<img width="1922" height="1047" alt="Logical conditions" src="https://github.com/user-attachments/assets/5852d733-c2a0-4ec4-aa4d-12e80036832d" />

These payloads manipulated backend SQL queries, allowing unauthorized access to database contents.
Log Analysis

Web server logs were analyzed from:

/opt/lampp/logs/access_log

The logs captured HTTP requests generated during the attack.

 Sample Log Evidence

GET /vulnerabilities/sqli/?id=1' OR '1'='1 HTTP/1.1
GET /vulnerabilities/sqli/?id=1' UNION SELECT user,password FROM users -- HTTP/1.1
<img width="1920" height="1051" alt="Access logs" src="https://github.com/user-attachments/assets/704cbd8c-38da-4f3b-92c0-5d37321952ed" />

These entries clearly show SQL Injection payloads embedded in URL parameters.

Indicators of Compromise (IOCs)

The following patterns were identified as indicators of SQL Injection attempts:

- "' OR '1'='1"
- "UNION SELECT"
- SQL comment usage ("--")
- Suspicious query parameters in GET requests
- Encoded payloads ("%27", "%20", etc.)
- Detection Approach
<img width="1920" height="1051" alt="Access logs 2" src="https://github.com/user-attachments/assets/57b2cb25-98b1-405e-849c-f8d0db018845" /> 

Detection was performed by filtering logs for suspicious SQL-related patterns.

Example Detection Command

grep -Ei "union|select|1=1|--" /opt/lampp/logs/access_log

This allows rapid identification of malicious requests within large volumes of traffic.
Security Insights

Even though the application was vulnerable and allowed successful exploitation, the attack was clearly visible in server logs.

The logs revealed:

- The exact payloads used
- The targeted endpoints
- The structure of malicious requests

This demonstrates that effective logging enables detection and investigation of attacks.
rom a defender’s perspective, SQL Injection attacks can be detected by:

- Monitoring web server logs for abnormal patterns
- Identifying SQL keywords within user input
- Tracking repeated suspicious requests
- Correlating activity from specific IP addresses

With proper integration into monitoring systems (SIEM), such patterns can trigger alerts and support incident response.

Security Gaps Identified

The system was vulnerable due to:

- Lack of input validation and sanitization
- Direct execution of user-controlled SQL queries
- Absence of prepared statements
- Weak application security controls

Additionally, while logging was present, no active monitoring or alerting mechanism was implemented.

Conclusion

This project demonstrates how offensive actions can be analyzed from a defensive perspective.

While SQL Injection enabled full database compromise, the same activity was clearly visible in server logs, making detection possible.

It highlights the importance of combining secure coding practices with effective monitoring to both prevent and detect attacks.
