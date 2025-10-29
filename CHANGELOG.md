# Changelog

All notable changes to the Claude Code Plugins will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.5.0] - 2025-10-29

### Added

#### AI-Accessibility Plugin (v1.0.0)

- `/accessibility-audit` command with `accessibility-auditor` agent and `Accessibility Audit` skill
  - **WCAG 2.1 and 2.2 compliance checking** with configurable conformance levels (A, AA, AAA)
  - **Interactive pre-audit configuration** - Select WCAG version, conformance level, and scope
  - **Scoped analysis** - Audit entire codebase or specific directories/files
  - **Comprehensive accessibility pattern detection**:
    - Semantic HTML structure and heading hierarchy
    - ARIA implementation and landmark regions
    - Keyboard navigation and focus management
    - Color contrast analysis for text and UI components
    - Form labels, instructions, and error handling
    - Alternative text for images and multimedia
    - Interactive component accessibility (buttons, links, modals, widgets)
    - Screen reader compatibility
    - Touch target sizes and mobile accessibility
  - **Timestamped audit reports** saved to `/docs/accessibility/{timestamp}-accessibility-audit.md`
  - **Severity-based findings** (Critical, High, Medium, Low) with file paths and line numbers
  - **WCAG compliance matrix** showing status for each applicable criterion
  - **Code remediation examples** with before/after comparisons for every finding type
  - **Prioritized remediation roadmap** (Phase 1-4) with effort estimates
  - **Hybrid Agent + Skill Architecture** for optimal context efficiency
  - **Includes "What This Plugin Is (and Isn't)" section** with workflow diagram

#### Marketplace Documentation

- **Added clarifying notices for audit plugins** to set proper expectations
  - Added "Note on Audit Plugins" section in main README after Available Plugins table
  - Clarifies that audit plugins (accessibility, security, performance) are developer-focused code analysis tools
  - Emphasizes these plugins complement (not replace) runtime testing tools and professional services
  - Provides guidance on proper workflow: early detection during development + validation with specialized tools

### Changed

#### AI-Security Plugin (v1.2.1)

- **Updated README with clarifying notices**
  - Added "What This Plugin Is (and Isn't)" section to set proper expectations
  - Added 4-phase security workflow diagram showing plugin's role in comprehensive security strategy
  - Clarifies plugin is for code-level analysis, complements (not replaces) runtime security tools and professional assessments

#### AI-Performance Plugin (v1.1.1)

- **Updated README with clarifying notices**
  - Added "What This Plugin Is (and Isn't)" section to set proper expectations
  - Added 4-phase performance workflow diagram showing plugin's role in comprehensive performance strategy
  - Clarifies plugin is for code-level analysis, complements (not replaces) APM platforms, load testing, and production monitoring

## [1.4.1] - 2025-10-24

### Fixed

#### AI-ADO Plugin (v1.2.1)

- **Fixed `/ado-timesheet-report` command**
  - Migrated from non-existent `wit_query` to `wit_my_work_items` MCP tool with client-side filtering
  - Fixed date range filtering to use date-only comparison (prevents time-of-day issues)
  - Fixed day-of-week calculation using platform-specific system commands (PowerShell on Windows, `date` on Mac/Linux)
  - Prevents off-by-one errors in current week date range calculations

### Changed

- Condensed changelog entries across all versions for improved readability while maintaining essential information

## [1.4.0] - 2025-10-23

### Added

#### AI-ADO Plugin (v1.2.0)

- `/ado-timesheet-report` command for generating flexible weekly timesheet reports
  - Flexible task filtering (closed only, worked on only, or both)
  - Multiple date field support (Closed Date or Changed Date)
  - Three grouping modes (by hierarchy, by date, or by date with hierarchy)
  - Three verbosity levels (ID & hours, with titles, or with descriptions)
  - Flexible week definitions (Monday-Sunday or Sunday-Saturday)
  - User filtering (current user or specific team member)
  - Summary statistics and formatted reports with emoji icons

## [1.3.0] - 2025-10-21

### Added

#### AI-ADO Plugin (v1.1.0)

- **AI-Powered Work Item Generation** for `/ado-create-feature`, `/ado-create-story`, and `/ado-create-task` commands
  - AI generates titles, descriptions, persona statements, and acceptance criteria
  - Review and confirmation workflow with override options
  - Analyzes CLAUDE.md for naming conventions and standards
  - Context-aware generation using parent work item details

- `/ado-log-story-work` command for rapid work logging to User Stories
  - Creates tasks with completed hours pre-populated
  - AI-powered or manual task generation
  - Automatic git commit hash detection
  - Optional placeholder task hour subtraction

#### AI-Security Plugin (v1.2.0)

- **Hybrid Agent + Skill Architecture** with `security-audit` skill for optimal context efficiency
  - Progressive disclosure design: metadata loads first, full skill loads only when needed
  - Interactive security audits without consuming context until relevant

#### AI-Performance Plugin (v1.1.0)

- **Hybrid Agent + Skill Architecture** with `performance-audit` skill for optimal context efficiency
  - Progressive disclosure design: metadata loads first, full skill loads only when needed
  - Interactive performance analysis without consuming context until relevant

#### AI-Plugins Plugin (v1.2.0)

- **Plugin Development Skills** for interactive plugin and skill creation
  - New `plugins-scaffold` and `skills-scaffold` skills
  - Auto-loads when creating plugins, manifests, commands, agents, skills, or hooks

#### AI-ADO Plugin (v1.1.0)

- **Azure DevOps Work Items Skill** for MCP-powered work item management
  - New `ado-work-items` skill enforcing proper hierarchy and naming conventions
  - HTML formatting guidance and best practices
  - Auto-loads when using Azure DevOps MCP server tools

### Changed

#### AI-Security Plugin (v1.2.0)

- Converted `security-auditor` agent to lightweight wrapper that loads `security-audit` skill in fresh context

#### AI-Performance Plugin (v1.1.0)

- Converted `performance-optimizer` agent to lightweight wrapper that loads `performance-audit` skill in fresh context

### Fixed

#### AI-ADO Plugin (v1.1.0)

- Fixed `/ado-create-story` to properly set acceptance criteria in dedicated field instead of Description field

## [1.2.0] - 2025-10-20

### Added

#### AI-Git Plugin (v1.1.0)

- `/git-init` command for automated .gitignore generation
  - Intelligent technology detection (Node.js, Python, .NET, Go, Rust, PHP, Ruby, Java, Docker, React/Next.js, Vue, Terraform)
  - Comprehensive pattern generation (80+ patterns based on tech stack)
  - Smart merge strategies with preview and confirmation workflow

#### AI-ADO Plugin (v1.0.0)

- `/ado-init` command for Azure DevOps configuration and MCP server setup
  - Interactive configuration for organization, project, team, area path, and iteration path
  - Automatic CLAUDE.md configuration with work item hierarchy standards
  - Optional `.mcp.json` creation for MCP server integration
  - Cross-platform support with PAT authentication guidance

- `/ado-create-feature` command for creating Feature work items
  - Interactive prompts for title and description with HTML formatting
  - Returns Feature ID with Azure DevOps web link

- `/ado-create-story` command for creating User Story work items
  - Interactive prompts for parent Feature, title, persona statement, acceptance criteria, and story points
  - HTML formatted with Given-When-Then acceptance criteria

- `/ado-create-task` command for creating Task work items
  - Interactive prompts for parent User Story, title, and hour estimate
  - Hour tracking with Original Estimate and Remaining Work fields

### Changed

#### AI-Git Plugin (v1.1.0)

- Renamed `/commit-push` command to `/git-commit-push` for consistency with `/git-init` naming convention

#### AI-Plugins Plugin (v1.1.0)

- Renamed `/scaffold-plugin` command to `/plugins-scaffold` for consistency with plugin naming convention

## [1.1.0] - 2025-10-19

### Added

#### AI-Security Plugin (v1.1.0)

- `/security-init` command for automated security settings initialization
  - Intelligent technology detection with comprehensive file denial patterns (40-60+ based on tech stack)
  - Smart merge strategies with preview and confirmation workflow

### Changed

#### AI-Security Plugin (v1.1.0)

- Enhanced `/security-audit` with pre-audit configuration check recommending `/security-init` for users with fewer than 4 deny rules

## [1.0.1] - 2025-10-17

### Added

#### AI-Plugins Plugin (v1.0.0)

- Initial release with `/scaffold-plugin` command for interactive plugin scaffolding (later renamed to `/plugins-scaffold`)
  - AI-assisted plugin creation with automatic directory structure, metadata, and marketplace registration

## [1.0.0] - 2025-10-17

### Added

#### Marketplace

- Initial release of Claude Code Plugins marketplace with `.claude-plugin/marketplace.json` metadata system
- Support for slash commands, agents, and skills

#### AI-Git Plugin (v1.0.0)

- `/commit-push` command for automated git commit and push (later renamed to `/git-commit-push`)
  - Intelligent commit message generation following repository conventions
  - Clean commit messages without AI attribution

#### AI-Security Plugin (v1.0.0)

- `/security-audit` command with `security-auditor` agent
  - OWASP Top 10 2021 compliance checking and vulnerability detection
  - Timestamped audit reports with risk scoring and remediation guidance

#### AI-Performance Plugin (v1.0.0)

- `/performance-audit` command with `performance-optimizer` agent
  - Performance anti-pattern detection and database optimization recommendations
  - Timestamped audit reports with impact scoring and optimization roadmap

### Documentation

- README.md, CLAUDE.md, individual plugin READMEs, and MIT license

[Unreleased]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.4.1...HEAD
[1.4.1]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.4.0...v1.4.1
[1.4.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.3.0...v1.4.0
[1.3.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.2.0...v1.3.0
[1.2.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/charlesjones-dev/claude-code-plugins-dev/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/charlesjones-dev/claude-code-plugins-dev/releases/tag/v1.0.0
