# AI-Security Plugin

**Comprehensive AI-powered security toolkit for Claude Code.** Intelligent security scanning, vulnerability detection, threat analysis, and secure development guidance.

---

## 🎯 What This Plugin Does

Provides a complete suite of AI-powered security tools including commands, agents, and skills that help you build secure applications, detect vulnerabilities, analyze threats, and maintain security best practices throughout your development lifecycle.

### 🔍 Important: What This Plugin Is (and Isn't)

**This is a DEVELOPER-FOCUSED CODE ANALYSIS TOOL** designed to help developers with codebase access identify security vulnerabilities during development.

#### ✅ What This Plugin IS:
- **Static code analysis tool** for developers working with source code
- **Vulnerability pattern detection** for OWASP Top 10 and common security issues
- **Developer education tool** with secure coding guidance and remediation examples
- **Complementary tool** to augment your security workflow
- **Early detection system** to catch vulnerabilities before they reach production

#### ❌ What This Plugin IS NOT:
- **NOT a replacement for runtime security monitoring** or application security platforms
- **NOT a penetration testing tool** (doesn't actively exploit vulnerabilities)
- **NOT a complete security solution** (catches code-level issues, not runtime/infrastructure issues)
- **NOT a substitute for dependency scanners** like Snyk or Dependabot (limited dependency analysis)
- **NOT a replacement for security audits** by professional security researchers
- **NOT a compliance certification tool** (doesn't provide legal compliance guarantees)

#### 🎯 How This Fits Into Your Security Workflow:

```
┌─────────────────────────────────────────────────────────────┐
│           Comprehensive Security Strategy                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Development Phase (Code-Level) ← THIS PLUGIN           │
│     • /security-audit - Static code analysis               │
│     • OWASP Top 10 pattern detection                       │
│     • Early vulnerability identification                    │
│                                                             │
│  2. Dependency Security                                    │
│     • Dependency scanning tools                            │
│     • CVE monitoring services                              │
│     • Software composition analysis                        │
│                                                             │
│  3. Runtime Security Testing                               │
│     • Dynamic application security testing (DAST)          │
│     • Runtime security monitoring                          │
│     • Infrastructure security scanning                     │
│                                                             │
│  4. Professional Security Assessment                       │
│     • Penetration testing by security professionals        │
│     • Security audits and code reviews                     │
│     • Compliance assessments                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Best Practice**: Use this plugin during development to catch code-level vulnerabilities early, then validate with dependency scanners, runtime security tools, and professional security assessments.

### Why Not Just Use `/security-review`?

Claude Code ships with a native `/security-review` command that provides basic security analysis. However, it has limitations:

❌ **No custom scan templates** - You can't define what the report should include
❌ **No output file control** - Results appear in chat but aren't saved to a specific location
❌ **No reproducibility** - Each scan produces different output formats
❌ **No standardization** - Can't ensure consistent audit documentation across projects

This plugin enhances security scanning by providing:

✅ **Custom scan templates** - Define exactly what your audit should cover
✅ **Controlled output location** - Reports saved to `/docs/security/{timestamp}-security-audit.md` (timestamp prevents overwrites)
✅ **Reproducible audits** - Same comprehensive format every time
✅ **Standardized documentation** - Consistent audit reports across all projects
✅ **Timestamped tracking** - Easy to compare audits over time
✅ **Enterprise-ready** - Professional audit documents suitable for compliance

**Use `/security-review` for:** Quick ad-hoc security checks during development

**Use `/security-audit` for:** Formal security audits, compliance documentation, and repeatable security assessments

## 📋 Available Commands

### `/security-init`

Initialize Claude Code security settings by automatically configuring `.claude/settings.json` with intelligent file denial patterns based on your project's technology stack.

**What it does:**

- 🔍 **Scans your project** to detect technologies (Node.js, Python, .NET, Go, Rust, PHP, Docker, etc.)
- 🛡️ **Builds comprehensive deny patterns** to prevent Claude Code from reading sensitive files:
  - Environment files (`.env`, `.env.*`)
  - Credentials and secrets (`credentials.json`, `secrets.yml`)
  - SSH keys and certificates (`*.pem`, `*.key`, `id_rsa`)
  - Cloud provider configs (`.aws/credentials`, `.gcp/*`)
  - Build artifacts (`node_modules`, `bin/`, `obj/`, `target/`, `vendor/`)
  - Version control and IDE files (`.git/`, `.vscode/`, `.idea/`)
- 🔄 **Smart merging** with existing settings (preserves your custom configurations)
- 📊 **Shows preview** before making changes
- ✅ **User confirmation** required before writing

**Before (manual):**

```
# Manually create .claude/settings.json
# Research what files should be denied
# Add patterns one by one
# Hope you didn't miss anything sensitive
```

**After (with ai-security plugin):**

```
/security-init
# ✨ AI detects your tech stack
# ✨ Builds comprehensive deny patterns (40-60+ patterns)
# ✨ Shows preview and asks for confirmation
# ✨ Merges with existing settings intelligently
# ✅ Security configured in seconds
# ⚠️  Restart Claude Code for settings to take effect
```

### `/security-audit`

Perform comprehensive security analysis on your codebase and generate a detailed audit report with vulnerability findings and remediation guidance.

**Note:** The `/security-audit` command will automatically check if you have proper file denial patterns configured. If you have fewer than 4 deny rules, it will recommend running `/security-init` first to ensure comprehensive protection.

**Before (manual):**

```
# Manual security review process
- Review each file for SQL injection patterns
- Check for XSS vulnerabilities
- Analyze authentication/authorization logic
- Review dependency vulnerabilities
- Check for hardcoded secrets
- Assess OWASP Top 10 compliance
- Write detailed findings document
- Create remediation plan
```

**After (with ai-security plugin):**

```
/security-audit
# ✨ AI analyzes entire codebase for vulnerabilities
# ✨ Identifies security risks across OWASP Top 10 categories
# ✨ Generates comprehensive audit report
# ✨ Provides code examples and remediation steps
# ✅ Report saved to /docs/security with timestamp
```

## 🤖 Available Agents

### `security-auditor`

Specialized agent that performs deep security analysis and generates comprehensive audit reports. Automatically invoked by `/security-audit` command.

---

## 📚 Available Skills

### `security-auditing`

---

## 🚀 Quick Start

### Installation

```
/plugin install ai-security@claude-code-plugins-dev
```

### Usage

```
# Step 1: Initialize security settings (recommended first step)
/security-init

# Step 2: Restart Claude Code (required for settings to take effect)
# Close and reopen Claude Code

# Step 3: Run a security audit
/security-audit

# Step 4: Review the generated report
# Located at: /docs/security/{timestamp}-security-audit.md
# Example: /docs/security/2025-10-17-143022-security-audit.md
```

---

## 💡 Features

### `/security-init` Command

#### Intelligent Technology Detection

Automatically detects your project's technology stack by scanning for indicator files:

- **Node.js**: `package.json`, `yarn.lock`, `pnpm-lock.yaml`
- **Python**: `requirements.txt`, `pyproject.toml`, `setup.py`, `poetry.lock`
- **.NET**: `*.csproj`, `*.sln`, `global.json`
- **Go**: `go.mod`, `go.sum`
- **Rust**: `Cargo.toml`, `Cargo.lock`
- **PHP**: `composer.json`, `composer.lock`
- **Ruby**: `Gemfile`, `Gemfile.lock`
- **Java**: `pom.xml`, `build.gradle`, `build.gradle.kts`
- **Docker**: `Dockerfile`, `docker-compose.yml`

#### Comprehensive File Denial Patterns

Builds an intelligent deny list with 40-60+ patterns covering:

**Security Essentials:**
- Environment files (all `.env` variants)
- Credentials and secrets files
- SSH keys and certificates (`.pem`, `.key`, `id_rsa`, etc.)
- Cloud provider configs (AWS, GCP, Azure)
- Database files (`.db`, `.sqlite`)

**Technology-Specific Patterns:**
- **Python**: `.venv/`, `__pycache__/`, `*.pyc`, `.pytest_cache/`, `dist/`, `*.egg-info/`
- **.NET**: `bin/`, `obj/`, `*.user`, `.vs/`, `TestResults/`
- **Node.js**: `node_modules/`, `.next/`, `.nuxt/`, `dist/`, `.turbo/`
- **Go**: `vendor/`
- **Rust**: `target/`
- **PHP**: `vendor/`
- **Ruby**: `vendor/bundle/`, `.bundle/`
- **Java**: `target/`, `*.class`, `.gradle/`

**Version Control & IDE:**
- `.git/**`, `.vscode/**`, `.idea/**`
- `.devcontainer/**`, `.github/workflows/**`

#### Smart Merge Strategies

When `.claude/settings.json` already exists, you can choose how to merge:

- **Deduplicate** (default): Remove duplicate patterns, add only new ones
- **Append**: Add all new patterns, keep any duplicates
- **Replace**: Completely replace existing deny section

#### Preview & Confirmation

Before making any changes, see:
- Technologies detected
- Current configuration (if exists)
- All new patterns grouped by category
- Total patterns before/after merge
- Merge strategy being used

### `/security-audit` Command

#### Comprehensive Vulnerability Detection

- **SQL Injection**: Identifies unsafe query construction patterns
- **Cross-Site Scripting (XSS)**: Detects unencoded user input in views
- **Authentication Bypass**: Analyzes JWT token validation and session management
- **Authorization Issues**: Finds missing or improper access controls
- **Hardcoded Secrets**: Locates API keys, passwords, and sensitive data in code
- **Insecure Direct Object References**: Identifies missing ownership validation

#### OWASP Top 10 2021 Compliance

Automatically assesses your codebase against all OWASP Top 10 categories:

- A01 - Broken Access Control
- A02 - Cryptographic Failures
- A03 - Injection
- A04 - Insecure Design
- A05 - Security Misconfiguration
- A06 - Vulnerable Components
- A07 - Identity & Authentication Failures
- A08 - Data Integrity Failures
- A09 - Security Logging Failures
- A10 - Server-Side Request Forgery

#### Architecture Security Assessment

- **Authentication & Authorization Analysis**: Reviews identity management patterns
- **Data Protection Analysis**: Checks encryption, validation, and encoding
- **Dependency Security**: Identifies vulnerable packages and outdated dependencies
- **Configuration Review**: Analyzes security headers, error handling, and settings

#### Detailed Reporting

Each finding includes:

- **Location**: Exact file path and line number
- **Risk Score**: Numerical severity rating (0-10)
- **Pattern Detected**: What vulnerability was identified
- **Code Context**: The problematic code snippet
- **Impact**: What could happen if exploited
- **Recommendation**: How to fix it
- **Fix Priority**: When it should be addressed

#### Code Remediation Examples

Reports include before/after code examples showing:

- Vulnerable code patterns
- Secure replacement code
- Explanation of the fix
- Best practices applied

---

## ⚙️ How It Works

The `/security-audit` command uses Claude Code's specialized **security-auditor agent** to perform comprehensive security analysis:

1. **Code Analysis**
   - Scans source files for security anti-patterns
   - Analyzes authentication and authorization flows
   - Reviews configuration files for misconfigurations
   - Checks dependency packages for known vulnerabilities

2. **Pattern Detection**
   - Identifies SQL injection vectors
   - Detects XSS vulnerabilities
   - Finds authentication bypass opportunities
   - Locates hardcoded secrets
   - Checks for missing authorization

3. **Report Generation**
   - Categorizes findings by severity (Critical, High, Medium, Low)
   - Provides exact locations with file paths and line numbers
   - Includes code examples and remediation guidance
   - Creates prioritized remediation roadmap
   - Saves timestamped report to /docs/security folder

The specialized agent brings deep security expertise and pattern recognition capabilities to identify vulnerabilities that might be missed by manual review.

---

## 🎓 Best Practices

### When to Run Security Audits

- ✅ Before production deployments
- ✅ After implementing authentication/authorization features
- ✅ When adding new API endpoints
- ✅ After integrating third-party libraries
- ✅ During security reviews and compliance checks
- ✅ As part of CI/CD pipeline (automated security gates)

### How to Use Audit Results

1. **Prioritize Critical & High findings** - Address these immediately
2. **Review code context** - Understand why each finding is a risk
3. **Apply remediation examples** - Use provided code fixes as templates
4. **Test fixes thoroughly** - Ensure security patches don't break functionality
5. **Re-audit after fixes** - Verify vulnerabilities are resolved

---

## ⏱️ Time Savings

**Per security audit:**

- Manual security review: ~4-8 hours (for medium-sized codebase)
- With security-audit: ~2-3 minutes (AI-powered analysis)

**Estimated savings:**

- Per comprehensive audit: Save ~4-8 hours
- Per month (2 audits): Save ~8-16 hours
- Per year: Save ~100+ hours

Plus reduced security incident risk and faster vulnerability remediation.

---

## 🔒 Security Expertise Built-In

This plugin embodies expert security knowledge across multiple domains:

### Vulnerability Detection

- OWASP Top 10 vulnerability patterns (2021 & 2025)
- Common authentication/authorization flaws
- SQL injection and XSS attack vectors
- API security vulnerabilities
- Cryptographic weaknesses

### Secure Development

- Secrets management best practices
- Secure coding standards (CERT, CWE)
- Input validation and sanitization
- Defense-in-depth strategies
- Security design patterns

### Threat Analysis

- Attack surface analysis
- Threat modeling methodologies (STRIDE, PASTA)
- Risk assessment frameworks
- Security architecture review

### Compliance & Standards

- OWASP ASVS (Application Security Verification Standard)
- CWE (Common Weakness Enumeration)
- NIST Cybersecurity Framework
- PCI-DSS, HIPAA, GDPR security requirements

---

## 🔧 Configuration

No configuration needed! The plugin works out of the box and generates comprehensive security reports automatically.

Reports are saved to: `/docs/security/{timestamp}-security-audit.md`

**Naming Format:** `YYYY-MM-DD-HHMMSS-security-audit.md` (e.g., `2025-10-17-143022-security-audit.md`)

This timestamp-based naming ensures multiple audits on the same day don't overwrite each other.

---

## 📦 Plugin Details

- **Name:** AI-Security
- **Type:** Comprehensive Security Toolkit
- **Features:**
  - Commands: `/security-init`, `/security-audit`
  - Agents: `security-auditor`
- **License:** MIT
- **Author:** Charles Jones

---

## ⚠️ Important Notes

### What This Plugin Does

- ✅ Static code analysis and pattern detection
- ✅ Architecture and configuration security review
- ✅ Dependency vulnerability assessment
- ✅ Security best practices validation
- ✅ Threat modeling and risk analysis
- ✅ Compliance checking and reporting
- ✅ Secure development guidance
- ✅ AI-assisted security education

### What This Plugin Doesn't Do

- ❌ Runtime penetration testing
- ❌ Network security scanning
- ❌ Dynamic application security testing (DAST)
- ❌ Exploit development or offensive security
- ❌ Replace manual security testing by experts
- ❌ Credential harvesting or malicious code creation

### Security & Ethics

This plugin is designed exclusively for **defensive security** purposes:

- Helps developers write secure code
- Identifies vulnerabilities early in the development lifecycle
- Educates teams on security best practices
- Supports compliance and audit requirements
- Prevents security incidents before they occur

**NOT for offensive security, exploit development, or malicious purposes.**

---

## 🤝 Contributing

Found a bug or have a suggestion? [Open an issue](https://github.com/charlesjones-dev/claude-code-plugins-dev/issues) or submit a pull request!

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

---

**Built with ❤️ for the Claude Code community**
