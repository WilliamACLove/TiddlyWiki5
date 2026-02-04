# CLAUDE.md - TiddlyWiki5 Project Guide

This file provides guidance for Claude Code when working with the TiddlyWiki5 codebase.

## Project Overview

TiddlyWiki5 is a non-linear personal web notebook implemented in JavaScript. It runs both as a single HTML file in browsers and as a Node.js application. The entire system is built on a plugin-based architecture where even core functionality is implemented as plugins.

## Quick Commands

```bash
# Development
npm run dev              # Start dev server at http://127.0.0.1:8080
npm test                 # Run full test suite (Jasmine + Playwright)
npm run lint             # Check ESLint rules
npm run lint:fix         # Auto-fix ESLint violations

# TiddlyWiki CLI
./tiddlywiki.js ./editions/tw5.com-server --listen     # Start server
./tiddlywiki.js --version                               # Show version
./tiddlywiki.js ./editions/test --verbose --build index # Run tests
```

## Directory Structure

- `boot/` - Bootstrap kernel (`boot.js` is the main entry point)
- `core/` - Core plugin with main functionality
  - `modules/` - JavaScript modules (widgets, filters, parsers, etc.)
  - `ui/` - UI component tiddlers
- `core-server/` - Server-specific modules (Node.js only, ES2023)
- `plugins/tiddlywiki/` - Official plugins (67+)
- `themes/tiddlywiki/` - Official themes
- `editions/` - Pre-configured wiki editions (dev, test, full, etc.)
- `languages/` - UI translations (30+ languages)
- `bin/` - Build and utility scripts

## Architecture

### Module System

Modules are self-describing JavaScript files with metadata headers:

```javascript
/*\
title: $:/core/modules/widgets/example.js
type: application/javascript
module-type: widget

Description of the module
\*/
(function(){
"use strict";
// Module code
})();
```

### Key Module Types

- `widget` - UI components
- `filter` - Filter operators for querying tiddlers
- `macro` - Macro implementations
- `parser` - Content parsers (wikitext, HTML, etc.)
- `saver` - Data persistence adapters
- `startup` - Initialization routines
- `indexer` - Data indexing (tags, fields, backlinks)
- `wikimethod` - Methods added to the wiki object

### Core Objects

- `$tw` - Global namespace containing all application data
- `$tw.wiki` - Main wiki instance with tiddler methods
- `$tw.modules` - Module registry
- `$tw.utils` - Utility functions

### Tiddler System

- Tiddlers are the core data unit with fields: `title`, `text`, `tags`, `type`, `modified`, `created`
- Tiddlers are immutable - changes create new instances
- Shadow tiddlers (prefixed with `$:/`) are system/theme tiddlers that can be overridden

## Code Style

ESLint enforces these rules:

- **Indentation:** Tabs (not spaces)
- **Quotes:** Double quotes
- **Semicolons:** Always required
- **Arrow functions:** Omit parens for single parameters
- **Language targets:**
  - ES2017 for browser code (most of codebase)
  - ES2023 for server code (`bin/` and `core-server/`)

## Testing

- **Unit tests:** Jasmine framework via `tiddlywiki/jasmine` plugin
- **E2E tests:** Playwright (Chromium + Firefox)
- **Test edition:** `editions/test/`
- Run with: `npm test`

## Contributing Guidelines

### PR Requirements

- Single feature per PR
- PR title: 50 chars max, imperative mood, capitalized, no period
  - Good: "Fix RSOE from filter operator errors"
  - Good: "Menu plugin: Include menu text"
- Explain the *why* and *what* in PR body
- Include screenshots for visual changes
- Follow the code style

### Important Notes

- CLA (Contributor License Agreement) required for contributions
- Open consultation issue for large changes before implementing
- Use Conventional Comments for code reviews
- Developer docs: https://tiddlywiki.com/dev

## Key Files

- `boot/boot.js` - Bootstrap kernel
- `core/modules/wiki.js` - Main Wiki API
- `core/modules/syncer.js` - Change tracking and sync
- `tiddlywiki.js` - CLI entry point
- `eslint.config.mjs` - ESLint configuration
