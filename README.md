## PassSec – Password Security Analyzer

PassSec is a cybersecurity-focused web application that analyzes password strength using entropy calculation, pattern detection, and real-world breach data.

It provides real-time feedback and highlights compromised passwords using external threat intelligence (HaveIBeenPwned API).

---

🌐 **Live Demo:** https://passsec.up.railway.app
The application runs live on Railway with automatic HTTPS — accessible from any browser, without local installation.

---

## How it works

- Calculates password entropy
- Detects character diversity (uppercase, lowercase, numbers, symbols)
- Estimates crack time using brute-force models
- Checks if password appears in known data breaches (HIBP API)
- Generates a security score (0–100)
  
---

## Screenshots - how two passwords have been analyzed

<img width="1536" height="1024" alt="Password strength analysis comparison" src="https://github.com/user-attachments/assets/25819533-976f-46dc-82b6-43f2c89ee615" />

---

> **Status: In Development** — Active project, constantly improving.
---

## About the Project

**Password Strength Checker** is a password strength analysis tool, built as an applied cybersecurity project. It analyzes passwords based on real security criteria, calculated mathematically, and provides detailed feedback to the user.

The project includes three modes of use: CLI (terminal), desktop GUI (tkinter) and web API (Flask), all based on the same central analysis engine.

Generated PDF report with design — PassSec logo, colored criteria table, visual score, SHA-256 hash, timestamp.

Real-time session statistics dashboard — no database required, no account required.
---

## Tech Stack

| Component | Technology |
|------------|-----------|
| Analysis Engine | Python 3.14 + `re` (regex) |
| Entropy Calculation | Shannon Entropy — `math.log2` |
| Hashing | SHA-256 via `hashlib` |
| API | Flask 3.1 + Flask-CORS |
| Desktop GUI | tkinter |
| Web Frontend | HTML5 · CSS3 · Vanilla JS |
| Compromised Passwords | rockyou.txt (14M+ passwords) |
| Security Standard | NIST SP 800-63B |

---

## What It Analyzes

The analysis engine runs **10 checks** on each password:

| # | Criterion | Type | Points |
|---|-----|-----|----|
| 1 | Check rockyou.txt (14M stolen passwords) | Auto-block | — |
| 2 | Progressive length (8 / 12 / 16 / 20+ chars) | Bonus | +10 → +30 |
| 3 | Uppercase `[A-Z]` | Bonus | +10 |
| 4 | Lowercase `[a-z]` | Bonus | +10 |
| 5 | Digits `[0-9]` | Bonus | +10 |
| 6 | Special symbols `[!@#$%...]` | Bonus | +20 |
| 7 | Repetitions (`aaa`, `111`) detected with regex | Penalty | -15 |
| 8 | Numeric sequences (`123`, `456`) | Penalty | -10 |
| 9 | Keyboard Pattern (`qwerty`, `asdf`) | Penalty | -10 |
| 10 | Maximum Diversity (all 4 types) | Bonus | +20 |

### Shannon Entropy Calculation

E = log2(A ^ L)
A = effective alphabet size
L = password length

NIST Reference: 60+ bits = Strong | 128+ bits = Crypto-grade

### Brute-Force Time Estimate
Based on **RTX 4090** — 164 billion hashes/second (real benchmark).

## API Endpoints

| Method | Endpoint | Description |
|--------|-----------|----------|
| `POST` | `/api/check` | Analyze a password |
| `POST` | `/api/report` | Generate `.txt` report |
| `GET` | `/api/download/<file>` | Download report |
| `GET` | `/api/status` | Health check server |

---

## | Vulnerability | Severity | Status |

- Complete security audit performed — identified and fixed 5 real vulnerabilities (path traversal, RCE via debug mode, XSS innerHTML, header injection, CORS wildcard) according to OWASP methodology
- Security headers — CSP, HSTS, X-Frame-Options
- Rate limiting - max 10 req/min per IP on /api/check, blocks automated attacks, flask-limiter(abuse prevention)
- Env variables (a mandatory standard in production)
- Deploy Railway with HTTPS
- Security logging - RotatingFileHandler

---

## CVE Security Scan

Dependency audit run with **pip-audit** — zero known vulnerabilities detected 
in all installed packages.

| Tool | Result | Date |
|------|-----|------|
| pip-audit | 0 CVE vulnerabilities | April 2026 |
| OWASP Manual Audit | 5 Vulnerabilities Fixed | April 2026 |

---

### API Integrations
- Integrated HaveIBeenPwned API with k-anonymity — password checking against 600M+ real breaches without the password ever leaving the server, with resilience to network failures

---

## Project Structure

password_strength/
├── checker.py # Main engine — regex, entropy, rockyou, HIBP
├── app.py # Flask API server — v3.1 secure
├── gui.py # tkinter GUI
├── password_checker.html # Dark-theme cybersecurity web interface
├── Procfile # Railway deployment configuration
├── requirements.txt # Python dependencies
├── README.md # Project documentation
├── venv/ # Virtual environment (local, not on GitHub)
├── data/
│ └── rockyou.txt # 14M compromised passwords (downloaded separately)
└── reports/ # Generated .txt reports (self-created)

---

## API Documentation

Full API documentation available at:

**[https://passsec.up.railway.app/api/docs](https://passsec.up.railway.app/api/docs)**

- All endpoints documented with request/response examples
- Error codes for each endpoint
- Rate limiting info
- HIBP k-anonymity model explained

---

## Bulk Password Verification

Analyzes up to **10 passwords in a single API request** — designed for
developers and companies integrating PassSec into their own systems.

**Use cases:**
- Companies checking employee passwords upon reset
- Web applications blocking weak passwords upon signup
- Security teams auditing password policies
- Developers integrating PassSec into existing applications

**Limits:** Max 10 passwords per request · 3 requests/min per IP · 500 characters per password

---

## CI/CD Pipeline — GitHub Actions

Automatic security pipeline run on every push to GitHub:

| Job | What it checks | When it runs |
|----|------------|--------------|
| Security Audit | pip-audit — zero CVEs in dependencies | On every push |
| API Integration Tests | /api/status responds "online", /api/check returns valid score | After Security Audit |

**If a test fails — the code does NOT go live on Railway.**

---

## Roadmap — Future Developments

> The project is in continuous development. The following features are planned:

### Authentication & User Accounts
- [ ] Registration / login system with email validation
- [ ] Account confirmation via activation link (email verification)
- [ ] Password reset via email with unique token with expiration (15 min)
- [ ] Two-Factor Authentication (2FA) via TOTP (Google Authenticator)
- [ ] Secured sessions with JWT (JSON Web Tokens)
- [ ] Logout from all devices (session invalidation)

### Database & Storage
- [ ] PostgreSQL integration for user storage
- [ ] Passwords stored EXCLUSIVELY as bcrypt hash (not simple SHA-256)
- [ ] Unique hash per password (anti rainbow-table attacks)
- [ ] Audit log — analysis history per user
- [ ] Row Level Security (RLS) — each user sees only their data

### Web Security
- [ ] Rate limiting — max 5 attempts/minute per IP (anti brute-force)
- [ ] Exponential backoff on failed login
- [ ] CAPTCHA after 3 consecutive failed attempts
- [ ] HTTPS required (SSL/TLS certified)
- [ ] Security headers: CSP, HSTS, X-Frame-Options
- [ ] CSRF protection on all forms
- [ ] Input sanitization — SQL Injection & XSS prevention

### API Integrations
- [ ] Automatic email notification if user password appears in a new breach
- [ ] Webhook support for integration into third-party applications

### Dashboard & Analytics
- [ ] Admin panel with usage statistics
- [ ] Graphs: score distribution, blocked passwords, detected breaches
- [ ] Export reports to PDF and CSV

### Penetration Testing (Final Pre-Release Phase)
- [ ] **OWASP Top 10** — full audit before deployment
- [ ] SQL Injection testing (SQLMap)
- [ ] XSS vulnerability scanning (DAST)
- [ ] Brute-force resistance testing
- [ ] Session management audit
- [ ] API security testing (Postman + Burp Suite)
- [ ] Dependency vulnerability scan (`pip audit', `safety')
- [ ] Docker container security scan

### Infrastructure & Deployment
- [ ] Full containerization with Docker Compose
- [ ] CI/CD pipeline (GitHub Actions)
- [ ] Environment variables for all secrets (not hardcoded)
- [ ] Cloud deployment (Railway / Render / AWS)
- [ ] Monitoring & alerting (uptime, errors, security)

---

## Security Principles Applied

- **Passwords stored as hash** — never in clear, not in reports
- **k-anonymity** — HIBP verification does not expose the password to the external server
- **Defense in depth** — 3-level validation: regex, dictionary, entropy
- **NIST SP 800-63B** — American reference standard for authentication
- **Principle of least privilege** — each component accesses the minimum necessary

---

## License

Personal project for educational and demonstration purposes. All rights reserved.

---

*Construit cu Python pe MacBook · VS Code · GitHub*
