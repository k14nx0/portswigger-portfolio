# 🔹 Lab 02: Username Enumeration Via Different Responses

## 📊 Vulnerability Classification
* **OWASP Top 10:** A07:2021 – Identification and Authentication Failures
* **Vulnerability Type:** Information Disclosure via Weak Error Handling / Brute-Force Automation
* **Severity:** Medium 🟡

---

## 📝 Technical Overview
The web application's authentication gateway contains an information leakage defect within its input validation error handling logic. The server responds with variations in page structural layout and string content depending on whether an entered username exists in the system database.

By capturing an authentication attempt and leveraging automated dictionary scripting, an external attacker can analyze subtle response anomalies to map out valid system users and execute a highly targeted brute-force password stuffing attack vector.

---

## 🚀 Step-by-Step Methodology

### 1️⃣ Phase 1: Traffic Capture & Parameter Mapping
1. Transmitted a fake authentication request parameter (`testuser` / `123456`) to the web interface log box.
2. Intercepted the transit packet pipeline stream mid-air via local loopback proxy listener layers within **Burp Suite Proxy**.
3. Captured the resulting data payload displaying a cleartext `POST /login HTTP/2` request pattern and shipped the package over into the automated scripting module (**Burp Suite Intruder**).

### 2️⃣ Phase 2: Automated Username Enumeration (Sniper Mode)
1. Cleared out default tag parameters inside the **Positions** sub-tab and anchored a target marker position strictly onto the username array value: `username=§testuser§`.
2. Swapped the payload repository configuration type to a `Simple list` and loaded a corporate list dictionary text block containing hundreds of candidate names.
3. Fired the automated attack stream. Upon completion, sorted the response matrix logs by clicking the **Length** column header.
4. Pinpointed a standalone text-length anomaly on row 60 (User: `americas`) returning a payload length profile size of **`3354`** bytes, while all other 100 dead attempts returned a flat baseline of `3352` bytes. This structural variance confirmed the username existed in the backend registry.

### 3️⃣ Phase 3: Targeted Password Brute-Forcing
1. Dropped the initial spreadsheet window and updated the primary Intruder positions dashboard line string to read: `username=americas&password=§123456§`.
2. Loaded the targeted passwords wordlist container into the Payloads array box.
3. Executed the second scripting matrix wave, monitoring the request streams until a successful **`302 Found` (Redirect)** HTTP status code response struck row 86 under the password payload value: **`access`**.
4. Authenticated manually into the web browser console app using the verified `americas:access` security credential token to claim the lab completion badge.

---

## 📸 Technical Portfolio Artifacts
* **Username Extraction Proof:** `![Intruder Username Length Anomaly](01-intruder-username-length-anomaly.png)`
* **Password Brute-Force Success:** `![Intruder Password 302 Redirect](02-intruder-password-302-redirect.png)`
* **Remediation Proof:** `![Lab Solved Victory Banner](03-lab-solved-victory-banner.png)`

---

## 🛡️ Defensible Security Remediation
* **Generic Error Messages:** Standardize all authentication failure screens to render generic, completely uniform error layouts regardless of data validation inputs (e.g., utilize a static message stating *"Incorrect username or password"*).
* **Network Rate Limiting:** Implement robust login rate-limiting defenses (Account Lockouts / IP Throttling) or enforce Captcha modules after multiple failed authentication triggers to nullify automated brute-force scripts completely.
