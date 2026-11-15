# Section 4: Risk Analysis & Threat Modeling Matrix
## SWYNEX Cyber Security Internship — Task 4 Capstone

---

## 1. Risk Assessment Framework

To quantify observed vulnerabilities and provide actionable priority guidance to engineering teams, this assessment employs two industry-standard risk measurement methodologies:
1. **Common Vulnerability Scoring System (CVSS v3.1)** for quantitative technical severity calculation.
2. **DREAD Threat Modeling Model** (Damage, Reproducibility, Exploitability, Affected Users, Discoverability) for qualitative business impact evaluation.

---

## 2. CVSS v3.1 Quantitative Scoring Breakdown

```text
+---------------------------------------------------------------------------------------------------------+
|                                    CVSS v3.1 VECTOR SCORING TABLE                                       |
+---------+-----------------------------------------+----------------------------------------+------------+
| Ref ID  | Finding Description                     | CVSS v3.1 Vector String                | Base Score |
+---------+-----------------------------------------+----------------------------------------+------------+
| SEC-01  | Admin API Missing Authentication        | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/  | 8.2 (HIGH) |
|         |                                         | I:L/A:N                                |            |
| WEB-01  | SQL Injection Authentication Bypass     | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/  | 8.6 (HIGH) |
|         |                                         | I:H/A:N                                |            |
| WEB-02  | Missing CSP & HSTS Response Headers     | CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/  | 5.4 (MED)  |
|         |                                         | I:L/A:N                                |            |
| SEC-02  | Missing Defense-in-Depth Headers        | CVSS:3.1/AV:N/AC:H/PR:N/UI:R/S:U/C:L/  | 4.3 (MED)  |
|         |                                         | I:L/A:N                                |            |
| WEB-03  | Search Input Parameter Handling         | CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:L/  | 4.1 (LOW)  |
|         |                                         | I:N/A:N                                |            |
+---------+-----------------------------------------+----------------------------------------+------------+
```

---

## 3. Qualitative DREAD Threat Modeling Matrix

Each finding is scored across 5 dimensions from 1 (lowest) to 10 (highest), resulting in a composite risk score out of 50:

| Finding Ref | Vulnerability Name | Damage (D) | Reproducibility (R) | Exploitability (E) | Affected Users (A) | Discoverability (D) | Total / 50 | Risk Tier |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **WEB-01** | SQL Injection Admin Bypass | 10 | 10 | 10 | 9 | 7 | **46** | **Tier 1 (Critical/High)** |
| **SEC-01** | Admin API Missing Authentication | 9 | 10 | 9 | 8 | 8 | **44** | **Tier 1 (Critical/High)** |
| **WEB-02** | Missing CSP & HSTS Headers | 6 | 6 | 5 | 6 | 5 | **28** | **Tier 2 (Medium)** |
| **SEC-02** | Missing Defense Headers (Task 1) | 5 | 6 | 5 | 5 | 5 | **26** | **Tier 2 (Medium)** |
| **WEB-03** | Search Input Encoding Review | 4 | 5 | 4 | 4 | 5 | **22** | **Tier 3 (Low)** |

---

## 4. Business Impact & Threat Scenarios

### Scenario A: Unauthorized Administrative Takeover via SQLi (WEB-01)
- **Threat Actor:** Unauthenticated remote attacker.
- **Attack Path:** Attacker navigates to `/rest/user/login` and submits `' OR 1=1--`. The backend concatenates this into the query, bypassing the password clause and authenticating as the first record in the database (`admin@juice-sh.op`).
- **Consequences:** Attacker obtains an administrator JWT, exports customer records, alters system configuration, and accesses restricted backend microservices.
- **Remediation Urgency:** **Immediate (P0)**.

### Scenario B: Sensitive Telemetry Leakage via Unprotected Admin API (SEC-01)
- **Threat Actor:** Script kiddie or competitor scraping public endpoints.
- **Attack Path:** Attacker issues automated GET requests to `/api/admin/dashboard` without credentials.
- **Consequences:** Immediate retrieval of operational metrics, resident records, adoption statistics, and internal counts.
- **Remediation Status:** **Successfully Remediated** via `@role_required("ADMIN")` middleware.

### Scenario C: Downgrade & Cross-Site Scripting Exposure (WEB-02 & SEC-02)
- **Threat Actor:** Adversary in a Coffee Shop / Public Wi-Fi network (Man-in-the-Middle).
- **Attack Path:** Without HSTS, attacker uses tools like `sslstrip` to intercept HTTP requests before redirecting to HTTPS. Without CSP, if any injection flaw exists elsewhere in the application, client browsers will execute malicious remote scripts.
- **Remediation Urgency:** **Short-Term (P1)**.
