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

### `Accessibility Audit`

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

---

## 📊 Example Audit Results

### Sample Finding

```markdown
#### A-001: Missing Alternative Text for Images

**Location**: `src/components/Hero.tsx:45-48`
**WCAG Criterion**: 1.1.1 Non-text Content (Level A)
**Severity**: Critical
**Pattern Detected**: Images without alt attributes

**Code Context**:
```tsx
<img src="/images/hero-banner.jpg" className="hero-image" />
```

**Impact**: Screen reader users cannot access image content. Fails WCAG 1.1.1.

**Remediation**:
```tsx
<img
  src="/images/hero-banner.jpg"
  alt="Team collaboration in modern office workspace"
  className="hero-image"
/>
```

**Fix Priority**: Immediate (within 24 hours)
```

### WCAG Compliance Matrix

| Criterion | Title | Status | Issues |
|-----------|-------|--------|--------|
| 1.1.1 | Non-text Content | ❌ Fail | 12 images missing alt text |
| 1.3.1 | Info and Relationships | ⚠️ Partial | Form labels, heading hierarchy |
| 1.4.3 | Contrast (Minimum) | ❌ Fail | 8 elements below 4.5:1 |
| 2.1.1 | Keyboard | ⚠️ Partial | Some widgets not keyboard accessible |
| 2.4.7 | Focus Visible | ❌ Fail | Focus indicators removed |
| 4.1.2 | Name, Role, Value | ❌ Fail | Multiple ARIA and form issues |

---

## ⚙️ How It Works

### Static Code Analysis

The audit uses static code analysis to identify accessibility patterns:

1. **File Discovery**: Scans configured scope for HTML, JSX, TSX, Vue, Angular templates
2. **Pattern Detection**: Identifies accessibility anti-patterns and violations
3. **WCAG Mapping**: Maps findings to specific WCAG success criteria
4. **Severity Assessment**: Prioritizes findings by user impact
5. **Report Generation**: Creates comprehensive markdown report with:
   - Executive summary with metrics
   - Severity-based findings with file paths/line numbers
   - WCAG compliance matrix
   - Code remediation examples (before/after)
   - Prioritized remediation roadmap (Phase 1-4)
   - Effort estimates and resource requirements

### Limitations

**What This Plugin Does:**
- ✅ Identifies semantic HTML and ARIA pattern violations
- ✅ Detects missing alt text and form labels
- ✅ Analyzes heading hierarchy and document structure
- ✅ Identifies color contrast issues in code
- ✅ Reviews ARIA implementation against specifications
- ✅ Provides WCAG compliance matrix and remediation guidance

**What This Plugin Doesn't Do:**
- ❌ Execute JavaScript or render pages in a browser
- ❌ Test actual screen reader behavior
- ❌ Measure real-time color contrast from rendered pages
- ❌ Test complex user interactions
- ❌ Replace manual testing with assistive technologies
- ❌ Guarantee 100% WCAG compliance (manual testing still required)

**Best Practice**: Use this plugin for comprehensive code-level audits, then validate with:
- Manual screen reader testing (NVDA, JAWS, VoiceOver)
- Automated browser tools (axe DevTools, WAVE, Lighthouse)
- User testing with people with disabilities

---

## 🎓 Best Practices

### When to Use This Plugin

✅ **Before production deployment** - Catch accessibility issues early
✅ **During feature development** - Ensure new features are accessible
✅ **For compliance audits** - Generate formal WCAG compliance documentation
✅ **When updating designs** - Verify color contrast and visual accessibility
✅ **For code reviews** - Include accessibility in PR review process
✅ **Quarterly accessibility reviews** - Track improvements over time

### How to Use Audit Results

1. **Start with Critical findings** - Address barriers preventing access first
2. **Follow the remediation roadmap** - Implement fixes in phases
3. **Test each fix** - Verify with screen readers and keyboard navigation
4. **Update team knowledge** - Share findings and patterns with team
5. **Track progress** - Use timestamped reports to measure improvement
6. **Establish patterns** - Create accessible component library from fixes

### Accessibility is Ongoing

- **Not a one-time fix** - Establish accessibility in development workflow
- **Requires manual testing** - Automated tools catch ~30-40% of issues
- **Include users with disabilities** - Most valuable testing comes from actual users
- **Document patterns** - Create reusable accessible components
- **Educate team** - WCAG training for developers and designers

---

## ⏱️ Time Savings

**Manual Accessibility Audit:**
- Document structure review: 2-4 hours
- ARIA implementation validation: 3-5 hours
- Keyboard navigation testing: 2-3 hours
- Color contrast calculations: 1-2 hours
- Form accessibility review: 2-3 hours
- Screen reader testing: 4-6 hours
- Report writing: 3-4 hours
- **Total: 17-27 hours**

**With AI-Accessibility Plugin:**
- Run `/accessibility-audit`: 3-5 minutes
- Review findings: 30-60 minutes
- Implement fixes: Varies by findings
- **Audit time saved: ~95%**

**Additional Benefits:**
- ✅ Consistent audit format across projects
- ✅ Timestamped reports for tracking progress
- ✅ WCAG compliance matrix included
- ✅ Code examples for every finding
- ✅ Prioritized remediation roadmap
- ✅ Reproducible audits

---

## 🔒 Accessibility Expertise Built-In

### WCAG 2.1 & 2.2 Knowledge
- All 59 WCAG 2.2 success criteria
- Level A, AA, and AAA requirements
- Understanding documents and techniques
- Common accessibility patterns

### Assistive Technology Understanding
- Screen reader behavior (NVDA, JAWS, VoiceOver)
- Keyboard navigation patterns
- Voice control considerations
- Screen magnification support

### Inclusive Design Principles
- Semantic HTML best practices
- ARIA Authoring Practices patterns
- Color contrast requirements
- Touch target sizing
- Responsive accessibility
- Mobile accessibility patterns

### Framework-Specific Knowledge
- React accessibility patterns
- Vue accessibility plugins
- Angular CDK accessibility
- HTML5 accessibility features
- Accessible component libraries (Radix, Reach UI, Chakra)

---

## 📦 Plugin Details

- **Name:** AI-Accessibility
- **Type:** Comprehensive Accessibility Toolkit
- **Features:**
  - Commands: `/accessibility-audit`
  - Agents: `accessibility-auditor`
  - Skills: `Accessibility Audit`
- **License:** MIT
- **Author:** Charles Jones

---

## ⚠️ Important Notes

### What This Plugin Does

- ✅ Provides comprehensive WCAG 2.1/2.2 compliance audits
- ✅ Identifies accessibility barriers in code
- ✅ Generates standardized audit reports with compliance matrix
- ✅ Provides code remediation examples
- ✅ Creates prioritized remediation roadmaps
- ✅ Supports configurable WCAG versions and levels
- ✅ Analyzes semantic HTML, ARIA, keyboard, contrast, forms, and more

### What This Plugin Doesn't Do

- ❌ Replace manual testing with screen readers
- ❌ Execute JavaScript or render browser content
- ❌ Test actual user interactions
- ❌ Guarantee 100% WCAG compliance (manual testing still required)
- ❌ Provide legal compliance certification

### Recommended Workflow

1. **Run `/accessibility-audit`** - Get comprehensive code-level analysis
2. **Implement high-priority fixes** - Address critical and high severity findings
3. **Manual testing** - Test with screen readers (NVDA, JAWS, VoiceOver)
4. **Browser tools** - Verify with axe DevTools, WAVE, Lighthouse
5. **User testing** - Test with actual users with disabilities (when possible)
6. **Iterate** - Run periodic audits to prevent regressions

---

## 🤝 Contributing

Found a bug or have a suggestion for improving accessibility detection? [Open an issue](https://github.com/charlesjones-dev/claude-code-plugins-dev/issues) or submit a pull request!

---

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

---

**Built with ❤️ for inclusive web development and the Claude Code community**
