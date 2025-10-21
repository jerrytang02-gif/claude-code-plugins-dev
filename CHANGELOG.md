# Changelog

All notable changes to the Claude Code Plugins will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.3.0] - 2025-10-21

### Added

#### AI-ADO Plugin (v1.1.0)

- **AI-Powered Work Item Generation** for all work item creation commands
  - `/ado-create-feature`, `/ado-create-story`, and `/ado-create-task` now support AI-powered generation mode
  - Users can choose between AI generation or manual input for each work item
  - AI generates professional content based on user description:
    - Features: Titles and comprehensive descriptions following naming conventions
    - User Stories: Titles, persona statements (As a... I want to... so that...), background information, and acceptance criteria (Given/When/Then format)
    - Tasks: Descriptive titles focused on work to be done
  - Review and confirmation workflow for all AI-generated content
  - Users can override any AI-generated field with custom text
  - AI analyzes CLAUDE.md configuration for naming conventions and standards
  - Context-aware generation retrieves parent work item details for better quality

#### AI-Security Plugin (v1.2.0)

- **Hybrid Agent + Skill Architecture** for optimal context efficiency
  - New `security-audit` skill (`skills/security-audit/SKILL.md`) with comprehensive security methodology
  - Interactive security audits when code is already in conversation context
  - Skill auto-loads when Claude detects security-related tasks
  - Full security expertise without consuming context until needed
  - Progressive disclosure design: metadata (~100 words) → full skill (~5k words) → only when relevant

#### AI-Performance Plugin (v1.1.0)

- **Hybrid Agent + Skill Architecture** for optimal context efficiency
  - New `performance-audit` skill (`skills/performance-audit/SKILL.md`) with comprehensive performance methodology
  - Interactive performance analysis when code is already in conversation context
  - Skill auto-loads when Claude detects performance-related tasks
  - Full performance expertise without consuming context until needed
  - Progressive disclosure design: metadata (~100 words) → full skill (~5k words) → only when relevant

#### AI-Plugins Plugin (v1.2.0)

- **Plugin Development Skills** for interactive plugin and skill creation
  - New `plugins-scaffold` skill (`skills/plugins-scaffold/SKILL.md`) for creating Claude Code plugins
  - New `skills-scaffold` skill (`skills/skills-scaffold/SKILL.md`) for creating Claude Code skills
  - Interactive guidance when creating plugins, manifests, commands, agents, skills, or hooks
  - Skills auto-load when Claude detects plugin development tasks
  - Comprehensive coverage of plugin architecture, marketplace setup, and best practices

#### AI-ADO Plugin (v1.1.0)

- **Azure DevOps Work Items Skill** for MCP-powered work item management
  - New `ado-work-items` skill (`skills/ado-work-items/SKILL.md`) for creating and managing ADO work items
  - Enforces proper work item hierarchy (Features → User Stories → Tasks)
  - HTML formatting guidance for descriptions and acceptance criteria
  - Naming convention support (decimal notation or descriptive names)
  - Story points and hour estimation best practices
  - Auto-loads when using Azure DevOps MCP server tools

### Changed

#### AI-Security Plugin (v1.2.0)

- **Converted `security-auditor` agent to lightweight wrapper** (reduced from ~2000 words to ~100 words)
  - Agent now loads the `security-audit` skill in fresh context
  - Eliminates context overhead in every conversation when agent isn't used
  - Enables flexible usage: skill for interactive work, agent for fresh context
  - Provides full 200K token budget when launching agent for large codebases

#### AI-Performance Plugin (v1.1.0)

- **Converted `performance-optimizer` agent to lightweight wrapper** (reduced from ~1500 words to ~100 words)
  - Agent now loads the `performance-audit` skill in fresh context
  - Eliminates context overhead in every conversation when agent isn't used
  - Enables flexible usage: skill for interactive work, agent for fresh context
  - Provides full 200K token budget when launching agent for large codebases

## [1.2.0] - 2025-10-20

### Added

#### AI-Git Plugin (v1.1.0)

- `/git-init` command for automated .gitignore generation and management
  - Intelligent technology detection (Node.js, Python, .NET, Go, Rust, PHP, Ruby, Java, Docker, React/Next.js, Vue, Terraform)
  - Comprehensive ignore pattern generation (80+ patterns based on tech stack)
  - Smart merge strategies for existing `.gitignore` files (Smart Merge, Append, Replace)
  - Preview and confirmation workflow before making changes
  - Base patterns for environment files, OS files, IDE files, and version control
  - Technology-specific patterns for dependencies, build outputs, testing, and caching
  - Lock file handling with user preference options
  - Pattern deduplication and custom pattern preservation
  - Organized sections with helpful comments

#### AI-ADO Plugin (v1.0.0)

- `/ado-init` command for Azure DevOps configuration and MCP server setup
  - Interactive configuration collection for organization, project, team, area path, and iteration path
  - Automatic CLAUDE.md configuration with Azure DevOps guidelines
  - Work item hierarchy standards (Features → User Stories → Tasks)
  - Naming convention support (decimal notation or descriptive names)
  - Optional `.mcp.json` creation for Microsoft Azure DevOps MCP server integration
  - Cross-platform support (Windows and Linux/Mac) with appropriate command configurations
  - Authentication setup guidance with direct links to Azure DevOps MCP repository
  - Personal Access Token (PAT) setup instructions for MCP server authentication
  - Security-conscious: Shows manual configuration snippet if `.mcp.json` already exists
  - Append-only strategy to prevent duplicate Azure DevOps sections

- `/ado-create-feature` command for creating Feature work items
  - Validates Azure DevOps configuration from CLAUDE.md before proceeding
  - Interactive prompts for feature title and high-level description
  - Automatic HTML formatting for description field with proper tags
  - Applies organization defaults (Area Path, Iteration Path, State)
  - Returns Feature ID with Azure DevOps web link for easy navigation
  - Next steps guidance for creating child User Stories

- `/ado-create-story` command for creating User Story work items
  - Creates User Stories as children of existing Features
  - Interactive prompts for parent Feature ID, title, persona statement, background, acceptance criteria, and story points
  - Structured description format with persona statement, background section, and acceptance criteria
  - Story Points estimation using Fibonacci sequence values
  - HTML formatting for improved readability in Azure DevOps
  - Given-When-Then format for acceptance criteria
  - Returns User Story ID with next steps for creating child Tasks

- `/ado-create-task` command for creating Task work items
  - Creates Tasks as children of existing User Stories
  - Interactive prompts for parent User Story ID, task title, and hour estimate
  - Lightweight task descriptions that reference parent User Story
  - Hour tracking with Original Estimate and Remaining Work fields
  - Retrieves parent story details for contextual task description
  - Returns Task ID with assignment and progress tracking guidance

### Changed

#### AI-Git Plugin (v1.1.0)

- Renamed `/commit-push` command to `/git-commit-push` for consistency with `/git-init` naming convention

#### AI-Plugins Plugin (v1.1.0)

- Renamed `/scaffold-plugin` command to `/plugins-scaffold` for consistency with plugin naming convention

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
- `/scaffold-plugin` command for interactive plugin scaffolding (later renamed to `/plugins-scaffold`)
  - Interactive plugin creation with AI assistance
  - Automatic directory structure generation
  - Metadata file generation (plugin.json)
  - Command template creation
  - Optional README and LICENSE generation
  - Automatic marketplace registration

## [1.0.0] - 2025-10-17

### Added

#### Marketplace

- Initial release of Claude Code Plugins marketplace
- Marketplace metadata system with `.claude-plugin/marketplace.json`
- Flat plugin directory structure for easy management
- Support for slash commands, agents, and skills

#### AI-Git Plugin (v1.0.0)

- `/commit-push` command for automated git commit and push workflow (later renamed to `/git-commit-push`)
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

[Unreleased]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.3.0...HEAD
[1.3.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.2.0...v1.3.0
[1.2.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/releases/tag/v1.0.0
