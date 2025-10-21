---
name: Security Audit
description: Guide for conducting comprehensive security audits of code to identify vulnerabilities. This skill should be used when reviewing authentication, input validation, cryptography, or API security.
version: 1.0.0
---

# Security Audit Skill

This skill provides elite security expertise for identifying and eliminating vulnerabilities before malicious actors can exploit them.

## When to Use This Skill

Invoke this skill when:
- Reviewing authentication and authorization mechanisms
- Auditing code for injection vulnerabilities (SQL, NoSQL, command, XSS)
- Validating input sanitization and data protection measures
- Assessing cryptographic implementations and key management
- Analyzing API security, rate limiting, and authorization controls
- Conducting security reviews of new features or code changes
- Auditing payment processing, file uploads, or sensitive data handling
- Investigating potential security vulnerabilities reported by users or tools

## Core Security Expertise

### 1. Authentication & Authorization Vulnerabilities

To identify authentication and authorization issues, examine:
- Password policies and storage mechanisms (bcrypt, argon2 vs plaintext)
- Session management and token expiration
- Authorization checks at every protected resource
- JWT token implementation (secret strength, expiration, algorithm)
- OAuth/SAML flows for common implementation errors
- Multi-factor authentication bypass opportunities

**Key Rules:**
- Never trust client-side authorization checks alone
- Every protected endpoint must verify both authentication AND authorization
- Session tokens should have appropriate timeouts and secure flags

### 2. Injection Attacks

To detect injection vulnerabilities, trace user input through:
- Database queries (SQL injection, NoSQL operator injection)
- System commands (command injection, path traversal)
- Template engines (template injection, SSTI)
- XML parsers (XXE injection)
- LDAP queries (LDAP injection)

**Key Rules:**
- User input must be validated, sanitized, and parameterized
- Never concatenate user input directly into queries or commands
- Use ORM/query builders with parameterization, not string concatenation

### 3. Input Validation & Sanitization

To validate input handling, check for:
- Whitelist-based validation on all user inputs
- Proper encoding for output contexts (HTML, JavaScript, URL, SQL)
- File upload restrictions (type, size, content validation)
- Mass assignment protection on data models
- Type validation to prevent coercion attacks

**Key Rules:**
- Validate on the server side, never trust client-side validation alone
- Use context-appropriate encoding when outputting user data
- Implement file upload validation beyond just file extension checks

### 4. Data Protection & Cryptography

To assess data protection, evaluate:
- Sensitive data identification and classification (PII, credentials, tokens)
- Encryption at rest (database fields, file storage)
- Encryption in transit (HTTPS enforcement, certificate validation)
- Cryptographic algorithm strength (avoid MD5, SHA1, DES, ECB mode)
- Key management and rotation procedures
- Timing attack vulnerabilities in comparison operations

**Key Rules:**
- Never store passwords in plaintext or with reversible encryption
- Use industry-standard libraries (not custom cryptography)
- Enforce HTTPS for all sensitive data transmission

### 5. API Security

To audit API security, review:
- Rate limiting implementation on all endpoints
- Object-level authorization (IDOR prevention)
- Excessive data exposure in API responses
- Security headers (CORS, CSP, HSTS, X-Frame-Options)
- API key rotation and secure storage
- Mass assignment vulnerabilities in request handlers

**Key Rules:**
- Implement rate limiting based on business requirements
- Return only the data the user is authorized to see
- Use security headers to enforce browser-side protections

### 6. Business Logic Vulnerabilities

To find business logic flaws, analyze:
- Race conditions in critical operations (TOCTOU issues)
- Price manipulation opportunities
- Privilege escalation paths through workflow abuse
- State transition validation
- Anti-automation controls (CAPTCHA, rate limiting)

**Key Rules:**
- Critical operations should be atomic and idempotent
- Validate state transitions, not just individual states
- Implement transaction-level integrity checks

## Audit Methodology

When reviewing code, follow this systematic approach:

1. **Threat Modeling**: Identify attack surfaces and potential threat actors
2. **Code Flow Analysis**: Trace data flow from user input to sensitive operations
3. **Vulnerability Scanning**: Systematically check for known vulnerability patterns
4. **Attack Simulation**: Think like an attacker - how would this be exploited?
5. **Defense Verification**: Validate that security controls are properly implemented
6. **Compliance Check**: Ensure adherence to standards (OWASP Top 10, PCI-DSS, GDPR)

## Output Format

Structure security audit reports with clear severity levels:

### 🔴 CRITICAL VULNERABILITIES
[Vulnerabilities allowing immediate system compromise, data breach, or authentication bypass]
- **Vulnerability**: [Name and type]
- **Location**: [File path:line numbers]
- **Impact**: [What an attacker can achieve]
- **Exploit Scenario**: [Step-by-step attack path]
- **Fix**: [Specific code changes required]
- **Priority**: IMMEDIATE

### 🟠 HIGH SEVERITY ISSUES
[Vulnerabilities with significant security impact requiring moderate effort]
- **Vulnerability**: [Name and type]
- **Location**: [File path:line numbers]
- **Impact**: [Potential damage]
- **Fix**: [Remediation steps]
- **Priority**: Within 24-48 hours

### 🟡 MEDIUM SEVERITY ISSUES
[Vulnerabilities requiring specific conditions or multiple steps to exploit]
- **Vulnerability**: [Name and type]
- **Location**: [File path:line numbers]
- **Impact**: [Risk assessment]
- **Fix**: [Recommended changes]
- **Priority**: Within 1 week

### 🟢 LOW SEVERITY / BEST PRACTICES
[Security improvements and hardening opportunities]
- **Issue**: [Description]
- **Recommendation**: [Enhancement suggestion]
- **Priority**: Next sprint

### ✅ SECURITY STRENGTHS
[Properly implemented security controls worth acknowledging]

## Severity Assessment Framework

When determining severity, apply these criteria:

- **CRITICAL**: Can an unauthenticated attacker access sensitive data or compromise the system?
- **HIGH**: Can an authenticated user escalate privileges or access other users' data?
- **MEDIUM**: Does exploitation require specific conditions or insider knowledge?
- **LOW**: Is this a defense-in-depth improvement or minor information disclosure?

## Examples

**Example 1: SQL Injection**

Bad approach:
```javascript
const query = `SELECT * FROM users WHERE email = '${userEmail}'`;
db.query(query);
```

Good approach:
```javascript
const query = 'SELECT * FROM users WHERE email = ?';
db.query(query, [userEmail]);
```

**Example 2: Password Storage**

Bad approach:
```javascript
user.password = md5(password);
```

Good approach:
```javascript
user.password = await bcrypt.hash(password, 12);
```

**Example 3: Authorization Check**

Bad approach:
```javascript
// Only checking authentication, not authorization
if (req.user) {
  return db.getOrder(req.params.orderId);
}
```

Good approach:
```javascript
// Check both authentication and authorization
if (req.user) {
  const order = await db.getOrder(req.params.orderId);
  if (order.userId !== req.user.id) {
    throw new ForbiddenError();
  }
  return order;
}
```

## Best Practices

1. **Assume Breach Mentality**: Design systems assuming attackers will gain some level of access. Implement defense-in-depth with multiple layers of security.

2. **Validate Context**: Consider the specific project architecture, technology stack, and business requirements when assessing vulnerabilities. A vulnerability's severity depends on context.

3. **Provide Actionable Fixes**: Every finding should include specific, implementable remediation steps with code examples where possible.

4. **Balance Security and Usability**: Recommend security measures that don't break functionality. Verify proposed fixes work with the existing architecture.

5. **Think Like an Attacker**: For each vulnerability, demonstrate concrete exploit scenarios to illustrate the real-world impact.

6. **Acknowledge Good Security**: Recognize properly implemented security controls to reinforce positive patterns and build trust.

## Quality Assurance Checklist

Before finalizing a security audit, verify:

- ✓ Have all user input points been identified and traced?
- ✓ Have sensitive data flows been traced end-to-end?
- ✓ Have both authenticated and unauthenticated attack vectors been considered?
- ✓ Are remediation recommendations specific and actionable?
- ✓ Have recommendations been validated to ensure they don't break functionality?
- ✓ Has business context and risk tolerance been factored into severity assessments?

## Context-Aware Analysis

When project-specific context is available in CLAUDE.md files, incorporate:

- **Project Architecture**: Understand security boundaries and trust zones
- **Technology Stack**: Identify framework-specific vulnerabilities and security features
- **Business Logic**: Recognize domain-specific security requirements
- **Compliance Requirements**: Note regulatory constraints (PCI-DSS, HIPAA, GDPR)
- **Existing Security Measures**: Build upon established security patterns

## Communication Guidelines

When reporting findings:
- Be direct and precise about security risks
- Use technical terminology accurately
- Provide concrete exploit scenarios to demonstrate impact
- Include code snippets showing both vulnerable and secure implementations
- Balance thoroughness with clarity - reports should be actionable
- Acknowledge properly implemented security controls
- Escalate critical findings immediately with clear urgency

Remember: The goal is not to criticize but to protect. Every vulnerability found and fixed is a potential breach prevented. Be thorough, be precise, and always think like an attacker while defending like a guardian.
