# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Shopify Skeleton theme - a minimalist, modular starter theme designed to be a foundation for custom development. It uses modern Shopify theme architecture with JSON templates and schema-based customization.

## Common Development Commands

### Theme Development
```bash
shopify theme dev              # Preview the theme locally with hot reload
shopify theme init              # Initialize a new theme based on this skeleton
shopify theme check             # Run Theme Check for Shopify best practices
```

## Architecture & Structure

### Template System
- Uses JSON templates (`templates/*.json`) for all pages instead of Liquid templates
- Each JSON template references sections that contain the actual Liquid markup
- Sections are modular and reusable across different templates

### Section Architecture
- Sections (`sections/`) contain both Liquid markup and schema definitions
- Schema settings should use:
  - CSS variables for single property changes
  - CSS classes for multiple property changes
- Sections can include `{% stylesheet %}` and `{% javascript %}` tags for component-specific assets

### Asset Management
- Critical CSS is separated in `assets/critical.css` and included in the `<head>`
- Component-specific styles use `{% stylesheet %}` tags within sections
- JavaScript uses `{% javascript %}` tags for automatic optimization

### Liquid Patterns
- Snippets (`snippets/`) contain reusable Liquid code
- Key snippets:
  - `css-variables.liquid` - Generates CSS variables from theme settings
  - `image.liquid` - Standardized image rendering with responsive support
  - `meta-tags.liquid` - SEO and social media meta tags

### Theme Configuration
- `config/settings_schema.json` - Defines theme-wide customization options
- `config/settings_data.json` - Stores current theme settings values
- Settings are accessed via `settings.variable_name` in Liquid

## Key Development Notes

### No Build Process
This theme has no Node.js dependencies or build process. All assets are processed by Shopify's theme pipeline using Liquid tags.

### Minimalist Philosophy
The theme intentionally excludes legacy features and maintains a minimal footprint. When adding features, consider if they align with this philosophy.

### Testing Changes
Always run `shopify theme check` before committing to ensure compliance with Shopify's best practices and theme requirements.