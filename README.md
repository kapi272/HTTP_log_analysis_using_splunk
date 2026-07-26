# Splunk Basics – HTTP Log Analysis

---

## 🎯 Objective

In this lab, learnt about:

- Learn how to ingest and analyze HTTP logs using Splunk.
- Detect client errors, server errors, and suspicious web activity.
- Identify large file transfers and suspicious URI access attempts.

---

## 🖥️ Lab Setup

- ✅ **Splunk:** Already installed and accessible.
- ✅ **Data Source:** JSON-formatted Zeek-style HTTP logs.
- 🌐 **Log File:** Download and upload to Splunk using the steps below.

📥 **Download HTTP Log file:** [https://raw.githubusercontent.com/0xrajneesh/30-Days-SOC-Challenge-Beginner/refs/heads/main/http_logs.json](http_logs.json)

---

## ⚙️ Steps to Upload HTTP Log into Splunk

1. Go to **Splunk Web → Settings → Data Inputs**.
2. Choose **Upload** and select `synthetic_zeek_http.json`.
3. Set **Source type:** `json` or create a new one `zeek:http`.
4. Set **Index:** Choose `main` or create a new index like `http_lab`.
5. Finish the upload and confirm indexing.

---

## 🔍 Lab Tasks

Use SPL queries to complete the following analysis.

---

### ✅ Task 1: Finding the top 10 endpoints generating web traffic

```spl
index=http_lab sourcetype="json"
| stats count by "id.orig_h"
| sort -count
| head 10
```

---

### ✅ Task 2: Counting the number of server errors (5xx) observed

```spl
index=http_lab sourcetype="json" status_code>=500 status_code<600
| stats count as server_errors
```

---

### ✅ Task 3: Identify User-Agents associated with possible scripted attacks

```spl
index=http_lab sourcetype="json" user_agent IN ("sqlmap/1.5.1", "curl/7.68.0", "python-requests/2.25.1", "botnet-check")
| stats count by user_agent
```

---

### ✅ Task 4: Find large file transfers (greater than 500 KB)

```spl
index=http_lab sourcetype="json" resp_body_len>500000
| table ts "id.orig_h" "id.resp_h" uri resp_body_len
| sort -resp_body_len
```

---

## 📸 Submission

top 10 endpoints generating web traffic [config.py](https://github.com/kapi272/HTTP_log_analysis_using_splunk/blob/c1a6a78bd2470d14e6aa065ff8076502e245ad51/Images/Task_01_top_10_endpoints_generating_web_traffic.png).
Counting the number of server errors (5xx) observed [config.py](https://github.com/kapi272/HTTP_log_analysis_using_splunk/blob/c1a6a78bd2470d14e6aa065ff8076502e245ad51/Images/Task_02_Count_the_number_of_server_errors_(5xx)_observed.png).
Identify User-Agents associated with possible scripted attacks [config.py](https://github.com/kapi272/HTTP_log_analysis_using_splunk/blob/c1a6a78bd2470d14e6aa065ff8076502e245ad51/Images/Task_03_Identify_User_Agents_associated_with_possible_scripted_attacks.png).
Find large file transfers (greater than 500 KB) [config.py](https://github.com/kapi272/HTTP_log_analysis_using_splunk/blob/c1a6a78bd2470d14e6aa065ff8076502e245ad51/Images/Task_04_Find_large_file_transfers_(greater_than_500_KB).png).
