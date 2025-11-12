# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains standalone web-based tools for email-related tasks. All tools are client-side applications built with vanilla HTML/CSS/JavaScript and require no backend or build process.

## Project Structure

The repository consists of three main components:

1. **Email Signature Generator** (`index.html`)
   - Interactive form-based signature builder
   - Generates HTML email signatures with embedded SVG icons (no external hosting)
   - Supports customization: colors, fonts, social media links, contact info
   - Copy-to-clipboard functionality for easy integration into email clients

2. **Email Spam Diagnostic Tool** (`spam-diagnostic.html`)
   - Analyzes email content to identify spam risk factors
   - Supports drag-and-drop email file parsing (.eml, .msg, .txt, .html)
   - Provides scoring system (0-100) with detailed diagnostics
   - Checks: spam trigger words, subject line quality, link analysis, sender reputation, compliance (CAN-SPAM, unsubscribe links)
   - Email file parser extracts From, Subject, and Body from .eml/.msg formats

3. **Shell Profile Configurations**
   - PowerShell profiles: `Documents/WindowsPowerShell/profile.ps1` and `Documents/PowerShell/Microsoft.PowerShell_profile.ps1`
   - CMD profile: `cmdProfile.cmd`
   - All provide custom prompt formatting (green path, newline before prompt)

## Technical Architecture

### No Build System
- Pure HTML/CSS/JavaScript with no dependencies
- No package.json, no npm scripts, no transpilation
- Files are opened directly in browser or deployed as-is

### Common Patterns

**UI Framework:**
- CSS Grid for responsive layouts (two-column desktop, single-column mobile)
- Gradient backgrounds (`#667eea` to `#764ba2`)
- Card-based UI components with consistent styling
- Breakpoint at 968px for mobile responsiveness

**JavaScript Patterns:**
- Event-driven updates with `input` and `change` listeners
- Data URI for embedded assets (SVG icons encoded as base64)
- Table-based HTML generation for email signatures (email client compatibility)
- Form validation and user feedback via notifications

**Email Signature Generator Specifics:**
- Generates signatures as HTML tables (not divs) for maximum email client compatibility
- All icons are embedded as data URIs to avoid external dependencies
- Uses `escapeHtml()` to prevent XSS attacks
- Clipboard API with execCommand fallback for older browsers

**Spam Diagnostic Tool Specifics:**
- Scoring algorithm sums penalties across 7 analysis categories
- Spam trigger word database (high-risk vs medium-risk words)
- Regex-based email file parser handles quoted-printable encoding
- File upload via both drag-and-drop and click-to-browse
- Risk levels: 0-19 (low), 20-49 (moderate), 50+ (high)

## Development Workflow

### Testing
Since there's no test suite, test manually by:
- Opening HTML files directly in multiple browsers (Chrome, Firefox, Safari, Edge)
- Testing email signatures in actual email clients (Gmail, Outlook, Apple Mail)
- For spam tool: test with various .eml files and edge cases (empty fields, malformed content)

### Editing
- Changes are reflected immediately on page refresh
- No build step required
- Maintain inline styles and scripts for portability

### Design Consistency
When adding new features:
- Use the established gradient color scheme (`#667eea`, `#764ba2`)
- Follow the card-based layout pattern
- Use form-group structure for inputs
- Maintain responsive grid layouts
- Include info-box components for user guidance

### Security Considerations
- Always use `escapeHtml()` when inserting user input into HTML
- Validate email addresses and URLs before processing
- Keep data processing client-side only (no server communication)

## Common Tasks

### Modifying the Signature Generator
- Icon updates: Replace base64 data URIs in the `icons` object
- Layout changes: Modify the table-based HTML generation in `generateSignature()`
- New fields: Add input in the form, capture in `inputs` object, incorporate into signature template

### Modifying the Spam Diagnostic Tool
- Adding spam keywords: Update `spamTriggerWords` database (high/medium arrays)
- New analysis checks: Create new `analyze*()` function, add to results object in `analyzeEmail()`
- Scoring adjustments: Modify penalty values in individual analysis functions
- File format support: Extend `parseEmailFile()` function

### Adding New Tools
Follow the established pattern:
1. Create standalone HTML file with embedded CSS/JS
2. Use consistent header styling and gradient background
3. Implement two-column card-based layout with responsive breakpoint
4. Include info boxes for user guidance
5. No external dependencies or build requirements
