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
- Performed security analysis of Windows Event Logs (Event Viewer)
- Investigated account creation events using Event ID 4720
- Identified suspicious user account "hacked" created by Administrator
- Correlated log data to establish attack timeline
- Detected password reset activity using Event ID 4724
- Assessed potential privilege abuse and persistence behavior


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
### Accessing the log file

<img width="909" height="735" alt="case1-01-log-files-access" src="https://github.com/user-attachments/assets/44848217-97cd-499b-b2c2-21c1042a99ac" />

### Viewing the log contents 

<img width="915" height="729" alt="case1-02-log-overview" src="https://github.com/user-attachments/assets/25978763-94a8-46a3-b509-5c5713811dce" />

### Identifying GET requests to /contact

<img width="894" height="748" alt="case1-03-get-requests-contact" src="https://github.com/user-attachments/assets/8cfade26-ea1d-4b98-91e2-7b40f5d5b472" />

### Investigating POST requests

<img width="907" height="752" alt="case1-04-post-requests-analysis" src="https://github.com/user-attachments/assets/d8e8e098-c9fd-42a5-aec6-341c944ec427" />

### Confirming suspicious POST activity

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


# 🧾 Case 2: Windows Event Logs Investigation

---

## 📌 Scenario

An organization reported a cyber attack where sensitive data was exfiltrated from a file server.  
The security team identified a compromised system and requested an investigation into attacker activity prior to the breach.

The objective is to analyze **Windows Security Event Logs** to determine:

- Account creation activity  
- Account modification behavior  
- Evidence of persistence or privilege abuse  

---

## 🎯 Objectives

- Identify newly created user accounts  
- Determine who created the account  
- Identify when the account was enabled  
- Detect password reset activity  
- Correlate events to understand attacker behavior  

---

## 🔍 Investigation Steps

### 1. Identify Newly Created User Account

```bash
Filter Security logs for Event ID 4720
```

**Result**
- New account created: **hacked**

---

### 2. Identify Who Created the Account

```bash
Review Event ID 4720 details
```

**Result**
- Account created by: **Administrator**

---

### 3. Determine When the Account Was Created

```bash
Review timestamp of Event ID 4720
```

**Result**
- Account creation date: **6/7/2024**

---

### 4. Investigate Password Reset Activity

```bash
Filter Security logs for Event ID 4724
```

**Result**
- Password reset activity detected: **Yes**

---

## 📊 Findings

- A new user account **“hacked”** was created on the system  
- The account was created by the **Administrator account**  
- The account was created on **6/7/2024**  
- A **password reset event (Event ID 4724)** was identified  
- The password reset targeted the **Administrator account**

---

## 🚨 Analysis

The sequence of events suggests suspicious activity:

- Creation of a new account (**hacked**)  
- Use of administrative privileges to create the account  
- Subsequent password reset activity involving a privileged account  

This behavior may indicate:

- Unauthorized account creation  
- Privilege misuse  
- Possible persistence mechanism by an attacker  

---

## 🧾 Evidence

### Accessing Security Logs
- Opened Event Viewer → Windows Logs → Security

<img width="927" height="829" alt="case2-01-accessing-security-logs" src="https://github.com/user-attachments/assets/01e97228-02c4-4765-a699-31997c2caac9" />


### Filtering Account Creation Events
- Filtered logs using **Event ID 4720**

<img width="925" height="827" alt="case2-02-filtered-logs-using-EventID-4720" src="https://github.com/user-attachments/assets/f2d21aae-f0db-41d0-babc-374f655d8b5e" />


### Reviewing Event Details
- Confirmed:
  - Account name: hacked  
  - Created by: Administrator

 <img width="925" height="828" alt="case2-03-reviewing-event-details" src="https://github.com/user-attachments/assets/d6687419-1bd1-4261-b38d-10d44b420a60" />


### Filtering Password Reset Events & Confirming Password Reset Activity
- Filtered logs using **Event ID 4724**
- Event message:
  > "An attempt was made to reset an account’s password."
- - Target account:
  - **Administrator**

<img width="924" height="829" alt="case2-04-filtered-logs-using-EventID-4724" src="https://github.com/user-attachments/assets/566d496d-e8a4-46ce-b997-a78bc94d4eb8" />


<img width="919" height="817" alt="case2-05-target-account" src="https://github.com/user-attachments/assets/5af85fbf-d070-4b4b-812a-accd61c7ef2f" />



---

## 🧠 Key Event IDs Used

- **4720** → User account created  
- **4724** → Password reset attempt  

---

## ✅ Conclusion

The investigation revealed that a new user account (**hacked**) was created using administrative privileges, followed by password reset activity on a privileged account.  

This sequence strongly suggests **potential unauthorized access and attacker persistence within the system**.
