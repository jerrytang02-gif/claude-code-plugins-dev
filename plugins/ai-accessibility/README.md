# AI-Accessibility Plugin

**Comprehensive AI-powered accessibility toolkit for Claude Code.** Intelligent accessibility scanning, WCAG compliance detection, inclusive design guidance, and accessible development best practices.

---

## 🎯 What This Plugin Does

Provides AI-powered accessibility auditing tools including commands, agents, and skills that help you build inclusive applications, detect accessibility barriers, achieve WCAG compliance, and maintain accessibility best practices throughout your development lifecycle.

### 🔍 Important: What This Plugin Is (and Isn't)

**This is a DEVELOPER-FOCUSED CODE ANALYSIS TOOL** designed to help developers with codebase access identify accessibility issues during development.

#### ✅ What This Plugin IS:
- **Static code analysis tool** for developers working with source code
- **Pattern detection engine** that identifies accessibility anti-patterns in HTML, JSX, CSS
- **Developer education tool** with WCAG knowledge and code remediation examples
- **Complementary tool** to augment your accessibility workflow
- **Early detection system** to catch issues before they reach production

#### ❌ What This Plugin IS NOT:
- **NOT a replacement for runtime accessibility testing services** that monitor live websites
- **NOT a browser-based visual testing tool** (doesn't render pages or test visual appearance)
- **NOT a complete accessibility solution** (catches code-level issues, not runtime/visual issues)
- **NOT a substitute for manual screen reader testing** with assistive technologies
- **NOT a replacement for user testing** with people with disabilities
- **NOT a compliance certification tool** (doesn't provide legal compliance guarantees)

#### 🎯 How This Fits Into Your Accessibility Workflow:

```
┌─────────────────────────────────────────────────────────────┐
│           Comprehensive Accessibility Strategy              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Development Phase (Code-Level) ← THIS PLUGIN           │
│     • /accessibility-audit - Static code analysis          │
│     • WCAG pattern detection in source files               │
│     • Early issue identification during development        │
│                                                             │
│  2. Runtime Testing Phase                                  │
│     • Automated browser testing tools                      │
│     • Continuous monitoring services for live sites        │
│     • Browser extensions for visual validation             │
│                                                             │
│  3. Manual Testing Phase                                   │
│     • Screen reader testing (NVDA, JAWS, VoiceOver)       │
│     • Keyboard-only navigation testing                     │
│     • Color contrast validation on rendered pages          │
│     • Touch target testing on actual devices               │
│                                                             │
│  4. User Testing Phase                                     │
│     • Testing with actual users with disabilities          │
│     • Usability validation                                 │
│     • Real-world accessibility verification                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Best Practice**: Use this plugin during development to catch code-level issues early, then validate with runtime testing tools, manual testing, and user testing before deployment.

### Why Use This Plugin?

Manual accessibility reviews are time-consuming, inconsistent, and prone to missing critical issues. Automated tools like WAVE, axe, and Lighthouse catch only ~30-40% of accessibility issues and don't provide comprehensive remediation guidance.

❌ **Manual accessibility review challenges:**
- No standardized methodology across projects
- Time-consuming manual checks
- Inconsistent findings format
- Missing WCAG compliance matrix
- Limited remediation guidance
- No historical tracking of improvements

This plugin provides:

✅ **Comprehensive WCAG audits** - Configurable for WCAG 2.1/2.2, Levels A/AA/AAA
✅ **Intelligent pattern detection** - Identifies semantic HTML, ARIA, keyboard, contrast issues
✅ **Standardized reports** - Consistent format with severity-based findings
✅ **WCAG compliance matrix** - Detailed assessment of applicable criteria
✅ **Code remediation examples** - Before/after examples for every finding
✅ **Prioritized roadmap** - Phased approach to accessibility improvements
✅ **Timestamped tracking** - Reports saved to `/docs/accessibility/{timestamp}-accessibility-audit.md`

**Use accessibility testing tools (WAVE, axe, Lighthouse) for:** Quick automated checks during development

**Use `/accessibility-audit` for:** Comprehensive WCAG compliance audits, formal accessibility documentation, and accessibility improvement roadmaps

## 📋 Available Commands

### `/accessibility-audit`

Perform comprehensive accessibility analysis on your codebase and generate a detailed WCAG compliance audit report with findings and remediation guidance.

**Interactive Configuration:**

Before starting the audit, the command will ask you:

1. **WCAG Version**: Choose between WCAG 2.1 or WCAG 2.2
2. **Conformance Level**: Choose A (minimum), AA (industry standard), or AAA (enhanced)
3. **Scope**: Choose entire codebase, specific directories, or specific files

**What it analyzes:**

- ✅ **Semantic HTML & Document Structure** - Heading hierarchy, landmarks, semantic elements
- ✅ **ARIA Implementation** - Roles, states, properties, landmark regions
- ✅ **Keyboard Navigation** - Tab order, focus management, keyboard traps, focus indicators
- ✅ **Color Contrast** - Text and UI component contrast ratios for WCAG compliance
- ✅ **Forms** - Label associations, error handling, required field indication
- ✅ **Alternative Text** - Images, icons, multimedia text alternatives
- ✅ **Interactive Components** - Buttons, links, modals, custom widgets
- ✅ **Screen Reader Support** - Accessible names, announcements, compatibility
- ✅ **Mobile Accessibility** - Touch target sizes, viewport scaling, orientation support

**Before (manual):**

```
# Manual accessibility audit process
- Review each page for heading hierarchy
- Check ARIA roles and attributes manually
- Test keyboard navigation flow
- Calculate color contrast ratios
- Review form label associations
- Check alt text on all images
- Test with multiple screen readers
- Write detailed findings document
- Create compliance matrix
- Develop remediation plan
```

**After (with ai-accessibility plugin):**

```
/accessibility-audit
# ✅ Choose WCAG version (2.1/2.2) and level (A/AA/AAA)
# ✅ Select scope (entire codebase or specific paths)
# ✨ AI analyzes codebase for accessibility barriers
# ✨ Identifies WCAG compliance issues with criterion references
# ✨ Generates comprehensive audit report with compliance matrix
# ✨ Provides code examples and remediation steps
# ✅ Report saved to /docs/accessibility with timestamp
```

## 🤖 Available Agents

### `accessibility-auditor`

Specialized agent that performs deep accessibility analysis and generates comprehensive WCAG compliance audit reports. Automatically invoked by `/accessibility-audit` command.

**Focus Areas:**
- Semantic HTML and document structure
- ARIA implementation validation
- Keyboard accessibility assessment
- Color contrast analysis
- Form accessibility evaluation
- Alternative text validation
- Interactive component review
- Screen reader compatibility

---

## 📚 Available Skills

### `accessibility-auditing`

Elite accessibility expertise skill that provides comprehensive WCAG knowledge, audit methodologies, and inclusive design guidance.

**Expertise Areas:**
- WCAG 2.1 and 2.2 conformance criteria (Levels A, AA, AAA)
- Semantic HTML and ARIA best practices
- Keyboard navigation patterns
- Color contrast requirements
- Form accessibility patterns
- Alternative text guidelines
- Interactive component accessibility (modals, dropdowns, tooltips)
- Screen reader compatibility
- Mobile and responsive accessibility

---

## 🚀 Quick Start

### Installation

```
/plugin install ai-accessibility@claude-code-plugins-dev
```

### Usage

```
# Step 1: Run an accessibility audit
/accessibility-audit

# Step 2: Answer configuration questions
# - WCAG Version: 2.1 or 2.2
# - Conformance Level: A, AA, or AAA
# - Scope: Entire codebase or specific paths

# Step 3: Review the generated report
# Located at: /docs/accessibility/{timestamp}-accessibility-audit.md
# Example: /docs/accessibility/2025-10-29-143022-accessibility-audit.md

# Step 4: Implement recommended fixes
# Follow the prioritized remediation roadmap (Phase 1-4)
```

---

## 💡 Features

### Configurable WCAG Assessment

**WCAG 2.1 vs 2.2:**
- **WCAG 2.1**: 50 success criteria across 13 guidelines
- **WCAG 2.2**: 59 success criteria (adds 9 new criteria for mobile, cognitive accessibility)

**Conformance Levels:**
- **Level A** (25 criteria) - Minimum accessibility, critical barriers
- **Level AA** (38 criteria) - Industry standard, often legally required
- **Level AAA** (61 criteria) - Enhanced accessibility, highest level

### Comprehensive Analysis Areas

#### Semantic HTML & Structure
- Heading hierarchy validation (h1-h6 without skipping levels)
- Semantic element usage (nav, main, footer, article, section)
- Landmark regions for screen reader navigation
- Logical reading order assessment

#### ARIA Implementation
- Valid ARIA roles, states, and properties
- Landmark roles (banner, navigation, main, complementary)
- Widget roles (button, checkbox, tab, dialog)
- Live regions for dynamic content
- ARIA labels and descriptions

#### Keyboard Accessibility
- Tab order follows logical flow
- All interactive elements keyboard accessible
- No keyboard traps detected
- Visible focus indicators (3:1 contrast minimum)
- Skip navigation links
- Focus management in modals/dialogs

#### Color Contrast
- Normal text: 4.5:1 minimum (AA), 7:1 (AAA)
- Large text: 3:1 minimum (AA), 4.5:1 (AAA)
- UI components: 3:1 minimum
- Information not conveyed by color alone
- Color blindness considerations

#### Form Accessibility
- Label associations (explicit/implicit)
- Fieldset/legend for grouped controls
- Required field indication
- Error identification and suggestions
- Accessible error messages (aria-describedby, aria-invalid)
- Autocomplete attributes

#### Alternative Text
- Descriptive alt text for informative images
- Empty alt for decorative images
- Complex image descriptions
- Icon button accessible names
- SVG accessibility
- Video captions and audio transcripts

#### Interactive Components
- Accessible names for all interactive elements
- Proper button vs link semantics
- Modal/dialog accessibility (focus trap, ESC key, aria-modal)
- Tooltip accessibility
- Dropdown/select accessibility
- Custom widget ARIA patterns

#### Mobile & Responsive
- Touch target size: 44×44px minimum (AA)
- No horizontal scrolling at 320px width
- Text can be zoomed to 200%
- Orientation not locked
- Content reflows at 400% zoom
