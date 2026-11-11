# Vulnerability Assessment Report (Read-Only Scope)

**Internship Track:** Cyber Security[cite: 5, 9]  
**Task Code:** FUTURE_CS_01[cite: 9]  
**Target Application:** `http://testasp.vulnweb.com`[cite: 3]  
**Auditor:** G. Jagadeeshwar Reddy  
**Contact:** +91 6304214258 | jagadeeshguduru006@gmail.com  
**Assessment Date:** November 2026  

---

## 1. Executive Summary
This project performs a non-intrusive, read-only security assessment on the web application hosted at `http://testasp.vulnweb.com`[cite: 3]. The objective is to identify transmission weaknesses, missing defensive HTTP response headers, infrastructure exposure, and session management flaws using passive scanning techniques and network service enumeration[cite: 12]. 

No active exploitation, brute force, or payload execution was performed, adhering strictly to ethical and read-only testing guidelines[cite: 12].

---

## 2. Assessment Scope & Methodology

### In-Scope[cite: 12]
- Public web application surface inspection (`http://testasp.vulnweb.com`)[cite: 3].
- HTTP response header and cookie attribute analysis[cite: 3, 4].
- Passive vulnerability analysis using OWASP ZAP (alert inspection without attack packets)[cite: 4, 12].
- Service banner and port enumeration using Nmap[cite: 12, 16].

### Out-of-Scope (Strictly Enforced)[cite: 12]
- Active vulnerability exploitation (e.g., executing SQL injection or cross-site scripting payloads)[cite: 12].
- Authentication bypass and credential brute-force attacks[cite: 12].
- Denial of Service (DoS/DDoS) testing[cite: 12].
- Invasive network or automated fuzzing[cite: 12].

---

## 3. Tools Utilized[cite: 12]
- **Google Chrome DevTools:** Inspected HTTP response headers, cookie flags, and transport encryption state[cite: 3, 12].
- **OWASP ZAP 2.x (Passive Mode):** Conducted non-intrusive security analysis to detect missing security headers and data leakage[cite: 4, 12].
- **Nmap 7.9x:** Verified open ports and fingerprinted web server software versions[cite: 12, 16].

---

## 4. Key Findings Summary

| # | Vulnerability / Issue | Risk Level[cite: 12] | CWE | Business & Technical Impact |
|---|---|---|---|---|
| 1 | Plaintext HTTP Communication (No TLS/HTTPS) | **Medium** | CWE-319 | Session tokens and credentials travel unencrypted over port 80, making data vulnerable to network interception (eavesdropping)[cite: 3, 16]. |
| 2 | Content Security Policy (CSP) Not Defined | **Medium** | CWE-693 | The browser does not restrict approved sources for scripts and assets, heightening exposure to client-side injection attacks[cite: 4]. |
| 3 | Missing Anti-Clickjacking Header (`X-Frame-Options`) | **Medium** | CWE-1021 | The site can be embedded within third-party `<iframe>` tags, leaving users vulnerable to clickjacking and UI redress attacks[cite: 4]. |
| 4 | Insecure Cookie Flags (Missing `HttpOnly` & `SameSite`) | **Low** | CWE-1004 | Cookies lack `HttpOnly` protection (readable via JavaScript) and omit `SameSite` flags, increasing cross-site leakage risks[cite: 4]. |
| 5 | Missing MIME-Sniffing Protection (`X-Content-Type-Options`) | **Low** | CWE-79 | Absence of `nosniff` enables older browsers to override declared content types, potentially treating benign content as executable code[cite: 4]. |
| 6 | Information Disclosure via Response Headers & Ports | **Low** | CWE-200 | Server broadcasts precise technical stack details (`Server: Microsoft-IIS/8.5`, `X-Powered-By: ASP.NET`) assisting attacker reconnaissance[cite: 3, 16]. |

---

## 5. Technical Evidence Artifacts

- **Target Overview:** `Evidence/Website_Overview/homepage.png`[cite: 17]
- **HTTP Header Analysis:** `Evidence/Security_Headers/devtools_headers.png`[cite: 3]
- **Passive Scanner Output:** `Evidence/OWASP_ZAP/zap_passive_alerts.png`[cite: 4]
- **Service Enumeration:** `Evidence/Nmap/nmap_scan.png`[cite: 16]

---

## 6. Strategic Remediation Roadmap[cite: 12]

1. **Immediate Actions (Transport Security):**
   - Bind a valid TLS/SSL certificate to port 443[cite: 16].
   - Configure HTTP-to-HTTPS permanent redirects (HTTP 301).
   - Implement `Strict-Transport-Security: max-age=31536000; includeSubDomains`.

2. **Defensive Hardening (Browser Policy Enforcement):**
   - Deploy `Content-Security-Policy: default-src 'self'`.
   - Set `X-Frame-Options: SAMEORIGIN` or utilize CSP `frame-ancestors 'self'`.

3. **Configuration & Hygiene:**
   - Enforce `; HttpOnly; SameSite=Lax; Secure` flags on all session cookies.
   - Deploy `X-Content-Type-Options: nosniff`.
   - Suppress verbose IIS headers (`Server`, `X-Powered-By`, `X-AspNet-Version`) in `web.config`[cite: 3].

---

## 7. Deliverable Location
The complete client-ready report is stored in the repository at:  
👉 **`Report/Vulnerability_Assessment_Report.pdf`**[cite: 12]