# Changelog

All notable changes to the Claude Code Plugins will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2025-10-19

### Added

#### AI-Security Plugin (v1.1.0)

- `/security-init` command for automated security settings initialization
  - Intelligent technology detection (Node.js, Python, .NET, Go, Rust, PHP, Ruby, Java, Docker)
  - Comprehensive file denial pattern generation (40-60+ patterns based on tech stack)
  - Smart merge strategies for existing `.claude/settings.json` configurations
  - Preview and confirmation workflow before making changes
  - Base security patterns for environment files, credentials, SSH keys, certificates, cloud configs, and database files
  - Technology-specific patterns for build artifacts, cache directories, and dependency folders

### Changed

#### AI-Security Plugin (v1.1.0)

- Enhanced `/security-audit` command with pre-audit configuration check
  - Automatically detects if `.claude/settings.json` has proper file denial rules
  - Warns users with fewer than 4 deny rules configured
  - Recommends running `/security-init` before performing comprehensive security audits

## [1.0.1] - 2025-10-17

### Added

#### AI-Plugins Plugin (v1.0.0)

- Initial release of AI-Plugins plugin for managing Claude Code plugins
- Plugin management and discovery capabilities

## [1.0.0] - 2025-10-17

### Added

#### Marketplace

- Initial release of Claude Code Plugins marketplace
- Marketplace metadata system with `.claude-plugin/marketplace.json`
- Flat plugin directory structure for easy management
- Support for slash commands, agents, and skills

#### AI-Git Plugin (v1.0.0)

- `/commit-push` command for automated git commit and push workflow
- Intelligent commit message generation following repository conventions
- Git status analysis and file staging
- Automatic detection of conventional commit patterns
- Push to remote with branch tracking
- Clean commit messages without AI attribution

#### AI-Security Plugin (v1.0.0)

- `/security-audit` command for comprehensive security analysis
- `security-auditor` agent with deep security expertise
- OWASP Top 10 2021 compliance checking
- Vulnerability detection for SQL injection, XSS, authentication issues, and more
- Timestamped security audit reports in `/docs/security/` directory
- Architecture security assessment capabilities
- Code remediation examples with before/after comparisons
- Risk scoring and prioritized remediation guidance
- Reproducible audit documentation for compliance requirements

#### AI-Performance Plugin (v1.0.0)

- `/performance-audit` command for comprehensive performance analysis
- `performance-optimizer` agent with deep performance optimization expertise
- Performance anti-pattern detection (N+1 queries, synchronous operations, memory issues)
- Database optimization recommendations with missing index detection
- Timestamped performance audit reports in `/docs/performance/` directory
- Architecture performance assessment (data access, application layer, infrastructure)
- Code optimization examples with before/after comparisons
- Impact scoring and prioritized optimization roadmap
- Expected performance improvement estimates

### Documentation

- README.md for marketplace overview and installation instructions
- CLAUDE.md for repository development guidelines
- Individual plugin README files with comprehensive documentation
- Plugin installation and usage instructions
- License information (MIT)

[Unreleased]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.1.0...HEAD
[1.1.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/releases/tag/v1.0.0
