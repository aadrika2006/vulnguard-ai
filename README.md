<div align="center">

<img src="https://img.shields.io/badge/Inspired%20by-Project%20Glasswing-orange?style=for-the-badge&logo=anthropic" alt="Inspired by Project Glasswing"/>

# 🛡️ VulnGuard AI

### AI-powered code vulnerability scanner for the open-source community

**Inspired by [Anthropic's Project Glasswing](https://www.anthropic.com/glasswing) — putting frontier AI security capabilities in the hands of every developer**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Claude API](https://img.shields.io/badge/Powered%20by-Claude%20Sonnet-blueviolet?logo=anthropic)](https://www.anthropic.com)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Hacktoberfest](https://img.shields.io/badge/Hacktoberfest-friendly-ff6f00)](https://hacktoberfest.com)

[Live Demo](#) · [Report a Bug](issues/new?template=bug_report.md) · [Request a Feature](issues/new?template=feature_request.md) · [Contributing Guide](CONTRIBUTING.md)

</div>

---

## 📖 What is VulnGuard AI?

VulnGuard AI is an **open-source, AI-powered code security scanner** that analyzes source code in real time and returns structured vulnerability reports — complete with severity ratings, attack scenarios, and concrete fix suggestions — powered by Anthropic's Claude API.

It was built as a **hackathon project** to demonstrate how the same frontier AI capabilities used by enterprise security teams (like those in Anthropic's Project Glasswing coalition) can be made **freely accessible to solo developers, open-source maintainers, and student teams**.

> **Context:** [Project Glasswing](https://www.anthropic.com/glasswing) is Anthropic's $100M initiative to use Claude to find and fix zero-day vulnerabilities across critical infrastructure — partnering with AWS, Apple, Google, Microsoft, and NVIDIA. VulnGuard AI is the open-source, community layer of that same mission.

### What it detects

| Vulnerability Class | Languages |
|---|---|
| SQL Injection (CWE-89) | Python, Java, PHP, Go |
| Cross-Site Scripting / XSS (CWE-79) | JavaScript, TypeScript |
| Path Traversal (CWE-22) | Python, Node.js, Bash |
| Hardcoded Credentials (CWE-798) | All languages |
| Command Injection (CWE-78) | Bash, Python, PHP |
| Insecure Deserialization (CWE-502) | Java, Python |
| Unsafe `eval()` usage | JavaScript, Python |
| Insecure Direct Object Reference | Python, Node.js |
| ...and more via AI reasoning | 5 languages + growing |

---

## ✨ Features

- **Multi-language support** — Python, JavaScript, SQL, Bash, Java (more coming)
- **Severity scoring** — CRITICAL / HIGH / MEDIUM / LOW / INFO ratings with a 0–100 security score
- **Attack scenarios** — plain-English descriptions of how an attacker would exploit each issue
- **Fix suggestions** — concrete code-level remediation advice, not just generic warnings
- **No data retention** — your code is sent to the Anthropic API and never stored by us
- **Zero install** — runs entirely in the browser, no server required
- **Hackathon-ready** — single HTML file, easy to fork and extend

---

## 🚀 Quick Start

### Option A — Run in browser (no install)

```bash
git clone https://github.com/YOUR_USERNAME/vulnguard-ai.git
cd vulnguard-ai
open index.html   # or just double-click the file
```

The app will prompt you to enter your Anthropic API key (stored only in your browser session, never sent anywhere else).

### Option B — Run with a local dev server

```bash
git clone https://github.com/YOUR_USERNAME/vulnguard-ai.git
cd vulnguard-ai
npx serve .
# Open http://localhost:3000
```

### Option C — Docker

```bash
docker build -t vulnguard-ai .
docker run -p 8080:80 vulnguard-ai
# Open http://localhost:8080
```

---

## 🔑 API Key Setup

VulnGuard AI uses the **Anthropic Claude API**. You need a free API key to run it.

1. Sign up at [console.anthropic.com](https://console.anthropic.com)
2. Create an API key under **API Keys**
3. Paste it into the app when prompted — it is stored only in `sessionStorage` and never leaves your browser

> ⚠️ **Never commit your API key to this repo.** The `.gitignore` already excludes `.env` files.

---

## 🗂️ Project Structure

```
vulnguard-ai/
│
├── 📄 index.html                  # Main app — single-file, self-contained
│
├── 📁 src/                        # Source files (for the extended version)
│   ├── 📁 components/
│   │   ├── Scanner.js             # Core scanning logic & Claude API calls
│   │   ├── ResultCard.js          # Vulnerability result card UI
│   │   ├── SummaryBar.js          # Score + severity summary strip
│   │   └── CodeEditor.js          # Code input + syntax hint area
│   │
│   ├── 📁 examples/               # Pre-loaded vulnerable code snippets
│   │   ├── python.js              # Python examples (SQLi, path traversal, etc.)
│   │   ├── javascript.js          # JS examples (XSS, eval, prototype pollution)
│   │   ├── sql.js                 # Raw SQL injection examples
│   │   ├── bash.js                # Shell injection examples
│   │   └── java.js                # Java deserialization examples
│   │
│   ├── 📁 prompts/
│   │   └── security-analyst.md    # The system prompt sent to Claude
│   │
│   └── 📁 utils/
│       ├── api.js                 # Anthropic API wrapper + error handling
│       ├── parser.js              # Parse + validate Claude JSON responses
│       └── severity.js            # Severity color mapping + score helpers
│
├── 📁 cli/                        # CLI tool for scanning local files
│   ├── scan.js                    # Entry point: node cli/scan.js ./myfile.py
│   ├── reporter.js                # Terminal output formatter
│   └── batch.js                   # Scan entire directories recursively
│
├── 📁 github-action/              # Drop-in GitHub Actions integration
│   ├── action.yml                 # Action metadata
│   ├── entrypoint.sh              # Shell entrypoint
│   └── scan-pr.js                 # PR diff scanner — comments inline on code
│
├── 📁 tests/
│   ├── 📁 fixtures/               # Known-vulnerable code files for testing
│   │   ├── vulnerable_python.py
│   │   ├── vulnerable_js.js
│   │   └── secure_python.py       # Should return 0 vulnerabilities
│   └── scanner.test.js            # Unit tests for parser + severity utils
│
├── 📁 docs/
│   ├── ARCHITECTURE.md            # How the scanning pipeline works
│   ├── PROMPT_ENGINEERING.md      # How the Claude prompt was designed
│   ├── ADDING_LANGUAGES.md        # Guide for adding a new language
│   └── API_REFERENCE.md           # If you expose VulnGuard as an API
│
├── 📁 .github/
│   ├── 📁 workflows/
│   │   ├── ci.yml                 # Run tests on every push
│   │   └── demo-deploy.yml        # Auto-deploy index.html to GitHub Pages
│   ├── 📁 ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
│
├── 📄 .env.example                # Example env vars (never commit .env itself)
├── 📄 .gitignore
├── 📄 Dockerfile
├── 📄 package.json
├── 📄 LICENSE                     # Apache 2.0
├── 📄 CONTRIBUTING.md
├── 📄 SECURITY.md                 # Responsible disclosure policy
└── 📄 README.md                   # This file
```

---

## 🔭 Roadmap

### v1.0 — Hackathon MVP ✅
- [x] Browser-based scanner (single HTML file)
- [x] Python, JavaScript, SQL, Bash, Java support
- [x] Severity scoring with Claude
- [x] Fix suggestions + attack scenarios
- [x] Pre-loaded vulnerable code examples

### v1.1 — Developer Tools
- [ ] **CLI tool** — `npx vulnguard scan ./src`
- [ ] **GitHub Action** — block PRs with CRITICAL vulnerabilities
- [ ] **VS Code Extension** — scan on save, inline warnings
- [ ] **PDF report export**

### v1.2 — Extended Language Support
- [ ] Go, Rust, PHP, C/C++, Ruby, TypeScript
- [ ] Infrastructure-as-Code (Terraform, Kubernetes YAML, Dockerfiles)
- [ ] `.env` file secret detection

### v2.0 — Team Features
- [ ] **Project dashboard** — scan entire repos via GitHub URL
- [ ] **Historical tracking** — security score over time
- [ ] **CI/CD integrations** — GitLab, Bitbucket, Jenkins
- [ ] **Compliance mapping** — map findings to OWASP Top 10, NIST, PCI-DSS

---

## 🤝 How to Contribute

We welcome contributions of all sizes. You don't need to be a security expert.

```bash
# 1. Fork this repo on GitHub

# 2. Clone your fork
git clone https://github.com/YOUR_USERNAME/vulnguard-ai.git
cd vulnguard-ai

# 3. Create a branch for your feature
git checkout -b feature/add-go-support

# 4. Make your changes, then commit
git add .
git commit -m "feat: add Go language support with examples"

# 5. Push and open a Pull Request
git push origin feature/add-go-support
```

### Great first issues for contributors
- 🌐 **Add a new language** — add examples + update the prompt in `src/prompts/`
- 🐛 **Add vulnerable code examples** — real-world CVE reproductions for the examples panel
- 🎨 **UI improvements** — dark mode toggle, syntax highlighting in the editor
- 📖 **Documentation** — improve ARCHITECTURE.md or write a tutorial blog post
- 🧪 **Tests** — add fixture files and assertions for the parser

See [CONTRIBUTING.md](CONTRIBUTING.md) for the full guide.

---

## 🔐 Security & Privacy

- **Your code is never stored.** Each scan is a stateless API call to Anthropic. Nothing is logged or persisted on our end.
- **API key safety.** Your Anthropic key lives only in your browser's `sessionStorage` and is cleared when you close the tab. Never commit it to version control.
- **Responsible disclosure.** If you find a security issue in VulnGuard AI itself, please email `security@yourdomain.com` instead of opening a public issue. See [SECURITY.md](SECURITY.md).

---

## 🧠 How It Works

```
User pastes code
       │
       ▼
Language is detected / selected
       │
       ▼
Code + language → Claude Sonnet via Anthropic API
  (with a structured security-analyst system prompt)
       │
       ▼
Claude returns JSON: { score, summary, vulnerabilities[] }
       │
       ▼
UI renders: severity cards, fix suggestions, attack scenarios
```

The key insight borrowed from Project Glasswing: **the same model capability that could generate exploits can be pointed at defense** — finding vulnerabilities faster than human reviewers, with reasoning about exploitability rather than just pattern matching.

See [docs/PROMPT_ENGINEERING.md](docs/PROMPT_ENGINEERING.md) for the full prompt design and why structured JSON output was chosen over free-text.

---

## 📚 Useful Resources

| Resource | Link |
|---|---|
| Project Glasswing (Anthropic) | https://www.anthropic.com/glasswing |
| Claude API Docs | https://docs.anthropic.com |
| OWASP Top 10 | https://owasp.org/www-project-top-ten |
| OpenSSF (open-source security foundation) | https://openssf.org |
| CWE — Common Weakness Enumeration | https://cwe.mitre.org |
| NVD — National Vulnerability Database | https://nvd.nist.gov |

---

## 📄 License

This project is licensed under the **Apache License 2.0** — see [LICENSE](LICENSE) for full text.

You are free to use, modify, and distribute this project — including commercially — as long as you include the original license and attribute the authors. You cannot use the "VulnGuard AI" name or Anthropic's name to endorse derivative products without permission.

---

## 🙏 Acknowledgements

- **[Anthropic](https://www.anthropic.com)** — for Project Glasswing, the Claude API, and the mission of making AI safety a public good
- **[OpenSSF](https://openssf.org)** and **[Alpha-Omega](https://alpha-omega.dev)** — for funding open-source security at scale
- **[The Linux Foundation](https://linuxfoundation.org)** — for anchoring Glasswing's open-source dimension
- All the open-source security researchers whose CVE write-ups informed our example library

---

<div align="center">

Built with ❤️ for the open-source security community

**[⭐ Star this repo if it helped you]()**  ·  **[🐛 Report a bug](issues)**  ·  **[💡 Suggest a feature](issues)**

</div>
