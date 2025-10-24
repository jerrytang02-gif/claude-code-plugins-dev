# Security Policy

## Supported Versions

We release patches for security vulnerabilities. Currently supported versions:

| Version | Supported          |
| ------- | ------------------ |
| 1.0.x + | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

If you discover a security vulnerability within this plugin marketplace or any of its plugins, please send an email at [https://charlesjones.dev/contact](https://charlesjones.dev/contact). All security vulnerabilities will be promptly addressed.

Please include the following information in your report:

- Description of the vulnerability
- Steps to reproduce the issue
- Affected plugin(s) and version(s)
- Potential impact
- Any suggested fixes (optional)

### What to Expect

- **Initial Response**: You will receive a response within 48 hours acknowledging your report
- **Updates**: We will keep you informed about the progress of addressing the vulnerability
- **Resolution**: Once the vulnerability is fixed, we will notify you and credit you in the release notes (unless you prefer to remain anonymous)

## Security Best Practices

When using plugins from this marketplace:

1. **Review Plugin Code**: All plugins are open source. Review the code before installation
2. **Keep Updated**: Regularly update plugins to receive security patches
3. **Use Security Plugin**: Install the `ai-security` plugin and run `/security-init` to configure secure defaults
4. **Report Issues**: If you notice suspicious behavior, report it immediately
5. **Sensitive Data**: Never commit sensitive data (API keys, passwords, etc.) when using git automation plugins

## Scope

This security policy covers:

- Plugin marketplace infrastructure and configuration
- Individual plugin commands and functionality
- Dependencies and third-party integrations
- Documentation and examples

## Security Features

This marketplace includes the **ai-security** plugin which provides:

- `/security-init`: Configure Claude Code to prevent reading sensitive files
- `/security-audit`: Comprehensive security scanning and vulnerability detection
- `security-auditor` agent: Automated security analysis

## Contact

For security-related questions or concerns:

- Email: [https://charlesjones.dev/contact](https://charlesjones.dev/contact)
- GitHub Issues: [Report a security concern](https://github.com/charlesjones-dev/claude-code-plugins-dev/issues) (for non-sensitive issues only)
