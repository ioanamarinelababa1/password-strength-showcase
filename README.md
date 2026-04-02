# 🛡️ Password Strength Checker v3.0
### Cybersecurity Project — Python · Flask · Regex · Entropy · SHA-256 

---

🌐 **Live Demo:** https://passsec.up.railway.app
Aplicația rulează live pe Railway cu HTTPS automat — accesibilă din orice browser, fără instalare locală.

---

> ⚠️ **Status: In Development** — Proiect activ, în continuă îmbunătățire.

---

## 📌 Despre Proiect

**Password Strength Checker** este un instrument de analiză a securității parolelor, construit ca proiect de cybersecurity aplicat. Analizează parole pe baza unor criterii reale de securitate, calculate matematic, și oferă feedback detaliat utilizatorului.

Proiectul include trei moduri de utilizare: CLI (terminal), GUI desktop (tkinter) și API web (Flask), toate bazate pe același motor central de analiză.

Raport generat tip PDF cu design — logo PassSec, tabel criterii colorat, scor vizual, hash SHA-256, timestamp.

---

## ⚙️ Stack Tehnic

| Component | Tehnologie |
|-----------|-----------|
| Motor analiză | Python 3.14 + `re` (regex) |
| Calcul entropie | Shannon Entropy — `math.log2` |
| Hashing | SHA-256 via `hashlib` |
| API | Flask 3.1 + Flask-CORS |
| GUI Desktop | tkinter |
| Frontend Web | HTML5 · CSS3 · Vanilla JS |
| Parole compromise | rockyou.txt (14M+ parole) |
| Standard securitate | NIST SP 800-63B |

---

## 🔍 Ce Analizează

Motorul de analiză rulează **10 verificări** pe fiecare parolă:

| # | Criteriu | Tip | Puncte |
|---|----------|-----|--------|
| 1 | Verificare rockyou.txt (14M parole furate) | Blocare automată | — |
| 2 | Lungime progresivă (8 / 12 / 16 / 20+ chars) | Bonus | +10 → +30 |
| 3 | Litere mari `[A-Z]` | Bonus | +10 |
| 4 | Litere mici `[a-z]` | Bonus | +10 |
| 5 | Cifre `[0-9]` | Bonus | +10 |
| 6 | Simboluri speciale `[!@#$%...]` | Bonus | +20 |
| 7 | Repetiții (`aaa`, `111`) detectate cu regex | Penalizare | -15 |
| 8 | Secvențe numerice (`123`, `456`) | Penalizare | -10 |
| 9 | Pattern tastatură (`qwerty`, `asdf`) | Penalizare | -10 |
| 10 | Diversitate maximă (toate 4 tipurile) | Bonus | +20 |

### Calcul Entropie Shannon

E = log2(A ^ L)
A = dimensiunea alfabetului efectiv
L = lungimea parolei

Referință NIST: 60+ biți = Puternică | 128+ biți = Crypto-grade

### Estimare Timp Brute-Force
Bazat pe **RTX 4090** — 164 miliarde hashes/secundă (benchmark real).

## 📡 API Endpoints

| Method | Endpoint | Descriere |
|--------|----------|-----------|
| `POST` | `/api/check` | Analizează o parolă |
| `POST` | `/api/raport` | Generează raport `.txt` |
| `GET` | `/api/download/<file>` | Descarcă raportul |
| `GET` | `/api/status` | Health check server |

---

| # | Vulnerabilitate | Severitate | Status |

- Realizat security audit complet — identificat și remediat 5 vulnerabilități reale (path traversal, RCE prin debug mode, XSS innerHTML, header injection, CORS wildcard) conform metodologiei OWASP
- Security headers — CSP, HSTS, X-Frame-Options
- Rate limiting - max 10 req/min per IP pe /api/check, blocheaza atacuri automate, flask-limiter(abuse prevention)
- Env variables(un standard obligatoriu in productie)
- Deploy Railway cu HTTPS

---

### 🔑 Integrări API
- Integrat HaveIBeenPwned API cu k-anonymity — verificare parolă față de 600M+ breach-uri reale fără ca parola să părăsească serverul, cu reziliență la erori de rețea

---

## 📁 Structura Proiectului

password_strength/
├── checker.py                  # Motor principal — regex, entropie, rockyou, HIBP
├── app.py                      # Flask API server — securizat v3.1
├── gui.py                      # Interfață grafică tkinter
├── password_checker.html       # Interfață web dark-theme cybersecurity
├── Procfile                    # Configurare deployment Railway
├── requirements.txt            # Dependințe Python
├── README.md                   # Documentație proiect
├── venv/                       # Virtual environment (local, nu pe GitHub)
├── data/
│   └── rockyou.txt             # 14M parole compromise (descărcat separat)
└── rapoarte/                   # Rapoarte .txt generate (auto-creat)

---

## 🗺️ Roadmap — Dezvoltări Viitoare

> Proiectul este în dezvoltare continua. Următoarele funcționalități sunt planificate:

### 🔐 Autentificare & Conturi Utilizatori
- [ ] Sistem de înregistrare / login cu validare email
- [ ] Confirmare cont prin link de activare (email verification)
- [ ] Resetare parolă prin email cu token unic cu expirare (15 min)
- [ ] Two-Factor Authentication (2FA) via TOTP (Google Authenticator)
- [ ] Sesiuni securizate cu JWT (JSON Web Tokens)
- [ ] Logout din toate dispozitivele (session invalidation)

### 🗄️ Bază de Date & Stocare
- [ ] Integrare PostgreSQL pentru stocarea utilizatorilor
- [ ] Parole stocate EXCLUSIV ca hash bcrypt (nu SHA-256 simplu)
- [ ] Salt unic per parolă (anti rainbow-table attacks)
- [ ] Audit log — istoricul analizelor per utilizator
- [ ] Row Level Security (RLS) — fiecare user vede doar datele sale

### 🌐 Securitate Web
- [ ] Rate limiting — max 5 încercări/minut per IP (anti brute-force)
- [ ] Exponential backoff la login eșuat
- [ ] CAPTCHA după 3 încercări consecutive eșuate
- [ ] HTTPS obligatoriu (SSL/TLS certificate)
- [ ] Headers de securitate: CSP, HSTS, X-Frame-Options
- [ ] Protecție CSRF pe toate formularele
- [ ] Input sanitization — prevenire SQL Injection & XSS

### 🔑 Integrări API
- [ ] Notificare email automată dacă parola utilizatorului apare într-un breach nou
- [ ] Webhook support pentru integrare în aplicații terțe

### 📊 Dashboard & Analytics
- [ ] Panou de administrare cu statistici de utilizare
- [ ] Grafice: distribuția scorurilor, parole blocate, breach-uri detectate
- [ ] Export rapoarte în PDF și CSV

### 🧪 Penetration Testing (Faza Finală Pre-Lansare)
- [ ] **OWASP Top 10** — audit complet înainte de deployment
- [ ] SQL Injection testing (SQLMap)
- [ ] XSS vulnerability scanning (DAST)
- [ ] Brute-force resistance testing
- [ ] Session management audit
- [ ] API security testing (Postman + Burp Suite)
- [ ] Dependency vulnerability scan (`pip audit`, `safety`)
- [ ] Docker container security scan

### 🐳 Infrastructure & Deployment
- [ ] Containerizare completă cu Docker Compose
- [ ] CI/CD pipeline (GitHub Actions)
- [ ] Environment variables pentru toate secretele (nu hardcodat)
- [ ] Deployment pe cloud (Railway / Render / AWS)
- [ ] Monitoring & alerting (uptime, erori, securitate)

---

## 🔒 Principii de Securitate Aplicate

- **Parole stocate ca hash** — niciodată în clar, nici în rapoarte
- **k-anonymity** — verificarea HIBP nu expune parola serverului extern
- **Defense in depth** — validare pe 3 niveluri: regex, dictionary, entropie
- **NIST SP 800-63B** — standard american de referință pentru autentificare
- **Principle of least privilege** — fiecare componentă accesează minim necesar

---

## 📄 Licență

Proiect personal în scop educațional și de demonstrație. Toate drepturile rezervate.

---

*Construit cu Python pe MacBook · VS Code · GitHub*
