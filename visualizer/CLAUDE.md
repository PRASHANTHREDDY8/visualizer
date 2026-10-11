# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of interactive, single-file HTML visualizations for studying DSA/LeetCode problems and system design concepts. Every output is a self-contained `.html` file (all CSS and JS inline) that opens directly in a browser with no build step.

## Architecture

- **`DSA/`** — Algorithm visualizations (dark theme). Each file has three sections: Visual Explanation, Step-by-Step Animation, Interactive Playground.
- **`System design/`** — Architecture/concept visualizations (light theme). Each file has eight sections: Architecture Diagram, Component Breakdown, Request Flow, Comparisons, Use Cases & Pitfalls, Best Practices, Scaling & Trade-offs, Interactive Playground.
- **`index.html`** — Landing page that links to all visualizations with card-grid layout, organized by domain.

There is no build system, no package manager, no bundler, and no server. Zero external dependencies — no CDN links, no imports. Vanilla JS only.

## Key Rules

- **Single-file constraint**: Every visualization is one `.html` file with all CSS in a `<style>` block and all JS in a `<script>` block. No shared stylesheets or JS modules.
- **Theming**: DSA files use dark backgrounds; System Design files use light backgrounds. Both use CSS custom properties (`:root` variables) for theming.
- **Naming**: Kebab-case filenames (`two-sum.html`, `url-shortener.html`).
- **After creating any new file**: Update `index.html` — add a `<a class="link-card">` entry in the correct section and increment the `<span class="count">` number.
- **The visualizer skill** (`.claude/skills/visualizer/skill.md`) is the authoritative specification for required content structure, section details, and quality bar.

## Development

No commands needed. Open any `.html` file directly in a browser to view it. To preview changes, refresh the browser.
