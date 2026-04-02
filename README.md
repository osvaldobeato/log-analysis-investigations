# Log Analysis Investigations

This repository contains SOC-style log analysis investigations focused on identifying suspicious activity using real-world security analysis techniques.

The cases include analysis of both web server logs and Windows event logs, demonstrating practical skills used by security analysts.

---

## Cases

### Case 1: Web Server Log Analysis
- Analyzed Apache access logs
- Identified suspicious POST activity
- Investigated abnormal HTTP responses

### Case 2: Windows Event Log Analysis
- (In progress)


# Web Server Log Analysis Lab

## Objective
Analyze a web server access log to identify suspicious activity, including abnormal GET and POST requests.

---

## Tools Used
- Linux (TryHackMe AttackBox)
- grep
- less

---

## Log File
- access.log

---

## Steps & Commands

### 1. View the log file
```bash
less access.log
```

---

### 2. Find GET requests to /contact
```bash
grep "GET /contact" access.log
```

### Result
- Last GET request to `/contact` came from:
```
10.0.0.1
```

---

### 3. Find POST requests
```bash
grep POST access.log
```

---

### 4. Find POST requests from specific IP
```bash
grep "172.16.0.1" access.log | grep POST
```

### Result
- Last POST request from `172.16.0.1`:
```
06/Jun/2024:13:55:44
```

---

### 5. Identify the URL of the POST request
```bash
grep "172.16.0.1" access.log | grep POST
```

### Result
- URL targeted:
```
/contact
```

## Evidence 
Accessing the log file
<img width="909" height="735" alt="case1-01-log-files-access" src="https://github.com/user-attachments/assets/44848217-97cd-499b-b2c2-21c1042a99ac" />
Viewing the log contents
<img width="915" height="729" alt="case1-02-log-overview" src="https://github.com/user-attachments/assets/25978763-94a8-46a3-b509-5c5713811dce" />
Identifying GET requests to /contact
<img width="894" height="748" alt="case1-03-get-requests-contact" src="https://github.com/user-attachments/assets/8cfade26-ea1d-4b98-91e2-7b40f5d5b472" />
Investigating POST requests
<img width="907" height="752" alt="case1-04-post-requests-analysis" src="https://github.com/user-attachments/assets/d8e8e098-c9fd-42a5-aec6-341c944ec427" />
Confirming suspicious POST activity
<img width="909" height="759" alt="case1-05-suspicious-post activity" src="https://github.com/user-attachments/assets/6119c965-1804-4553-95a3-bfe92fe1b663" />



---

## Findings
- Multiple requests to `/contact` endpoint
- Several HTTP errors (404, 500)
- POST request activity from internal IP `172.16.0.1`
- Behavior suggests possible misuse of web form

---

## Investigation Summary (5 W’s)

### Who
- Source IP involved in suspicious activity:
  - 172.16.0.1

### What
- Suspicious POST request activity targeting web application
- Repeated requests to `/contact` endpoint
- Multiple HTTP error responses (404, 500)

### When
- Last suspicious POST request:
  - 06/Jun/2024:13:55:44

### Where
- Target endpoint:
  - /contact
- Log source:
  - Apache web server (access.log)

### Why
- Behavior suggests possible probing or misuse of web form functionality
- Repeated errors and POST requests indicate abnormal usage patterns

---

## Conclusion
The analysis identified suspicious POST activity targeting the `/contact` page along with repeated failed requests. This may indicate probing or automated interaction with the web application.

---

## Skills Demonstrated
- Log analysis
- Pattern searching with grep
- Identifying suspicious network activity
- Basic incident investigation
