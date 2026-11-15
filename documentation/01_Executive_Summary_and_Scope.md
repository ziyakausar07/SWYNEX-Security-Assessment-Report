# Section 1: Executive Summary & Assessment Scope
## SWYNEX Cyber Security Internship — Task 4 Capstone

---

## 1. Context & Purpose

In modern digital ecosystems, security cannot be guaranteed through isolated perimeter defenses alone. A resilient defensive posture requires continuous, multi-tiered evaluations encompassing endpoint application controls, network transport integrity, and identity and access governance.

This report consolidates the security findings obtained during the 30-day **SWYNEX Cyber Security Internship** undertaken by **Ziyakausar J. Tamboli** (`SWX-2026-002678`). The assessment synthesizes three core hands-on assignments:
- **Task 1:** Foundational Web Security & API Access Control Remediation
- **Task 2:** Network Traffic Dissection & Protocol Security Analysis
- **Task 3:** Web Application Vulnerability Review (OWASP Top 10)

The primary goal of Task 4 is to integrate these separate technical workstreams into a cohesive, professional **Defensive Security Assessment Report** ready for executive presentation and technical implementation.

---

## 2. Assessment Scope & Boundaries

The evaluation was conducted across three distinct operational environments:

### Target Asset 1: Orphanage Management System (Task 1)
- **Frontend URL:** `https://orphanage-management-system-iota.vercel.app/`
- **Backend API URL:** `https://orphanagemanagementsystem.onrender.com/`
- **In-Scope Endpoints:**
  - `GET /api/admin/dashboard`
  - Client authentication and role verification flows
  - HTTP security response headers (`Strict-Transport-Security`, `Content-Security-Policy`, `X-Content-Type-Options`)
- **Out of Scope:** Third-party cloud hosting infrastructure (Vercel, Render), payment gateways, and brute-force denial of service.

### Target Asset 2: Controlled Host Network Interface (Task 2)
- **Platform:** Windows 11 Workstation with Wireshark v4.6.x
- **Interfaces Captured:** Local Wi-Fi Network Adapter (`Wi-FiFH69W3`, `Wi-FiPOLDX3`, `Wi-FiCIHYX3`)
- **In-Scope Protocol Layers:**
  - Layer 2: Ethernet II framing, MAC conversation statistics
  - Layer 3: IPv4 and IPv6 network layer routing
  - Layer 4: TCP three-way handshake (`SYN`, `SYN-ACK`, `ACK`), connection state flags (`FIN`, `RST`), UDP conversations
  - Layer 7: TLS 1.2 and TLS 1.3 cryptographic handshakes, Client Hello / Server Hello, SNI inspection, and stream reassembly
- **Out of Scope:** Live interception of unauthorized third-party network segments or active network packet injection.

### Target Asset 3: OWASP Juice Shop Containerized Sandbox (Task 3)
- **Target URL:** `http://localhost:3000` / `127.0.0.1:3000`
- **Platform:** OWASP Juice Shop (Node.js & Express single-page application)
- **In-Scope Testing Vectors:**
  - Security Misconfiguration (HTTP response headers, CORS settings)
  - Cross-Site Scripting (Reflected input handling on product search)
  - SQL Injection (Authentication bypass on `/rest/user/login`)
  - Authentication Mechanics (Credential validation & user enumeration resistance)
  - Broken Access Control (Session separation on `/rest/user/whoami`)
- **Out of Scope:** External hosting environments, infrastructure DoS, or automated vulnerability scanners that cause database corruption.

---

## 3. Rules of Engagement & Data Protection

All assessment procedures strictly adhered to the following ethical guidelines:
1. **Zero Production Disruption:** All offensive-style verification checks were executed solely in designated, isolated local environments or controlled staging instances.
2. **Sanitization of Evidence:** To uphold data privacy standards, all sensitive production keys, plain-text passwords, and personal identifying information (PII) were redacted prior to archiving in evidence repositories.
3. **Reproducibility:** Every reported finding includes exact reproduction steps and defensive verification commands to enable reliable engineering re-testing.
