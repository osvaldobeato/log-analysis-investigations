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
