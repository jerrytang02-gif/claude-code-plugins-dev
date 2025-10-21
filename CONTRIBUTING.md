# Contributing to Claude Code Plugins for Developers

Thank you for your interest in contributing to this Claude Code plugins marketplace! This document provides guidelines and instructions for contributing.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Ways to Contribute](#ways-to-contribute)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Plugin Development Guidelines](#plugin-development-guidelines)
- [Submitting Contributions](#submitting-contributions)
- [Review Process](#review-process)
- [Community](#community)

## Code of Conduct

This project follows a simple code of conduct:

- Be respectful and inclusive
- Provide constructive feedback
- Focus on what's best for the community
- Show empathy towards other contributors

## Ways to Contribute

You can contribute to this project in several ways:

### 1. Create New Plugins

Develop new plugins that extend Claude Code's capabilities for developers. Ideal plugins:
- Solve common development workflow problems
- Automate repetitive tasks
- Integrate with popular developer tools
- Follow security best practices

### 2. Improve Existing Plugins

Enhance existing plugins by:
- Adding new commands or features
- Fixing bugs
- Improving documentation
- Optimizing performance

### 3. Report Issues

Help improve the project by:
- Reporting bugs with detailed reproduction steps
- Suggesting new features or enhancements
- Identifying documentation gaps

### 4. Improve Documentation

Contribute to:
- Plugin README files
- Usage examples
- Command documentation
- This contributing guide

## Getting Started

### Prerequisites

1. **Install Claude Code**: [Download here](https://www.claude.com/product/claude-code)
2. **Familiarize yourself with the official documentation**:
   - [Plugins Guide](https://docs.claude.com/en/docs/claude-code/plugins)
   - [Plugins Reference](https://docs.claude.com/en/docs/claude-code/plugins-reference)
   - [Plugin Marketplaces](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)

### Setting Up Your Development Environment

1. **Fork and clone the repository**:
   ```bash
   git clone https://github.com/charlesjones-dev/claude-code-plugins-dev.git
   cd claude-code-plugins-dev
   ```

2. **Initialize security settings** (recommended):
   ```bash
   /security-init
   ```
   Restart Claude Code after running this command.

3. **Add the marketplace locally for testing**:
   ```bash
   /plugin marketplace add file://C:/path/to/your/local/clone
   ```

## Development Workflow

### Creating a New Plugin

#### Option 1: Using the Scaffolding Tool (Recommended)

Use the ai-plugins scaffolding tool for interactive plugin creation:

```bash
/plugins-scaffold
```

This will guide you through:
- Plugin naming and metadata
- Category selection
- Command creation
- Documentation generation
- Automatic marketplace registration

#### Option 2: Manual Creation

1. **Create plugin directory structure**:
   ```
   plugins/
     your-plugin-name/
       .claude-plugin/
         plugin.json          # Plugin metadata
       commands/
         command-name.md      # Command implementations
       README.md             # Plugin documentation
   ```

2. **Define plugin metadata** in `.claude-plugin/plugin.json`:
   ```json
   {
     "name": "your-plugin-name",
     "version": "1.0.0",
     "description": "Brief description of your plugin",
     "author": {
       "name": "Your Name",
       "url": "https://your-website.com"
     },
     "repository": "https://github.com/charlesjones-dev/claude-code-plugins-dev",
     "license": "MIT",
     "keywords": ["keyword1", "keyword2"]
   }
   ```

3. **Register the plugin** in `.claude-plugin/marketplace.json`

4. **Create command files** in `commands/` directory as markdown files

5. **Add plugin documentation** in `README.md`

### Updating Existing Plugins

When adding features or fixing bugs, follow this workflow:

1. **Update plugin version** in `plugins/{plugin-name}/.claude-plugin/plugin.json`
   - Follow [Semantic Versioning](https://semver.org/):
     - PATCH (0.0.x): Bug fixes
     - MINOR (0.x.0): New features (backward-compatible)
     - MAJOR (x.0.0): Breaking changes

2. **Update marketplace registry** in `.claude-plugin/marketplace.json`
   - Match version and description with plugin.json

3. **Update CHANGELOG.md**:
   - Add version section with release date
   - Document changes following [Keep a Changelog](https://keepachangelog.com/) format

4. **Update README.md**:
   - Update version in Available Plugins table
   - Document new commands or features

5. **Update plugin README** (`plugins/{plugin-name}/README.md`):
   - Document new functionality
   - Add usage examples

## Plugin Development Guidelines

### Plugin Structure

Follow this standard structure:

```
plugins/your-plugin-name/
    .claude-plugin/
        plugin.json           # Required: Plugin metadata
    commands/
        command-1.md          # Command implementation
        command-2.md
    README.md                 # Required: Plugin documentation
    LICENSE                   # Optional: If different from repo license
```

### Command Best Practices

When creating commands (markdown files in `commands/`):

1. **Clear Title**: Use a descriptive H1 heading
2. **Description**: Explain what the command does
3. **Instructions Section**: Include `## Instructions` heading
4. **Step-by-step**: Provide clear, actionable steps
5. **Constraints**: Specify what NOT to do (e.g., commit message requirements)
6. **Examples**: Include usage examples where helpful

Example structure:
```markdown
# Command Name

Brief description of what this command does.

## Instructions

1. First step
2. Second step
3. Third step

IMPORTANT: [Any critical constraints or requirements]
```

### Metadata Guidelines

Ensure consistency across all metadata files:

- **Matching names**: Same plugin name in plugin.json and marketplace.json
- **Matching versions**: Keep versions synchronized
- **Matching descriptions**: Consistent descriptions across files
- **Keywords**: Use relevant, searchable keywords
- **Author info**: Complete author information

### Testing Your Plugin

Before submitting:

1. **Test locally**:
   - Add your local repo as a marketplace
   - Install your plugin
   - Test all commands thoroughly

2. **Verify metadata**:
   - Check plugin.json is valid JSON
   - Verify marketplace.json is updated correctly
   - Ensure all versions match

3. **Test documentation**:
   - Verify README renders correctly
   - Check all links work
   - Ensure examples are accurate

## Submitting Contributions

### Pull Request Process

1. **Create a feature branch**:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes** following the guidelines above

3. **Commit your changes**:
   - Use descriptive commit messages
   - Follow conventional commit format if applicable (e.g., `feat:`, `fix:`, `docs:`)
   - Do NOT include Claude Code attribution in commits

4. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open a Pull Request**:
   - Use a clear, descriptive title
   - Explain what changes you made and why
   - Reference any related issues
   - Include testing steps

### Pull Request Checklist

Before submitting, ensure:

- [ ] Plugin follows the standard directory structure
- [ ] plugin.json includes all required metadata
- [ ] Plugin is registered in marketplace.json
- [ ] All versions are consistent across files
- [ ] README.md is comprehensive and accurate
- [ ] Commands are well-documented
- [ ] CHANGELOG.md is updated (for existing plugins)
- [ ] All commands have been tested
- [ ] No sensitive information (API keys, secrets) is included
- [ ] Code follows security best practices

## Review Process

### What to Expect

1. **Initial Review**: A maintainer will review your PR within 7 days
2. **Feedback**: You may receive requests for changes or clarifications
3. **Iteration**: Make requested changes and update your PR
4. **Approval**: Once approved, your PR will be merged
5. **Release**: Your contribution will be included in the next release

### Review Criteria

Pull requests are evaluated on:

- **Code Quality**: Well-structured, maintainable code
- **Documentation**: Clear, comprehensive documentation
- **Security**: No security vulnerabilities or risks
- **Usefulness**: Provides value to the developer community
- **Compatibility**: Works with current Claude Code version
- **Consistency**: Follows project conventions and style

## Community

### Getting Help

- **Documentation**: Check [CLAUDE.md](CLAUDE.md) for project-specific guidance
- **Issues**: Search existing issues or create a new one
- **Official Docs**: Consult [Claude Code documentation](https://docs.claude.com/en/docs/claude-code/plugins)

### Staying Updated

- Watch this repository for updates
- Check the [changelog](CHANGELOG.md) for recent changes
- Follow release announcements

### Recognition

All contributors will be recognized in:
- Pull request acknowledgments
- Release notes (for significant contributions)
- You're credited as author in plugin.json
- Active contributors may become maintainers

## License

By contributing to this project, you agree that your contributions will be licensed under the MIT License, the same license as this project.

---

Thank you for contributing to Claude Code Plugins for Developers! Your contributions help make development workflows more efficient for the entire community.
