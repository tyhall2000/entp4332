# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A small collection of standalone, single-file HTML demos comparing prompt quality. Each `SessionN_*` folder is an independent artifact — same task ("build a tip splitter"), different prompt given to an AI, no shared code between them:

- `Session2_BadPrompt/index.html` — result of a vague/minimal prompt. Simpler feature set (bill amount, tip presets 10/15/18/20/25%, custom tip, people stepper, per-person total).
- `Session2_DetailedPrompt/index.html` — result of a detailed prompt. More features (uneven split with per-person extras, round-up-to-dollar, copy-to-clipboard summary) and a distinct "receipt" visual theme.

There is no build system, package manager, server, or test suite — each `index.html` is fully self-contained (inline `<style>` and `<script>`, no external dependencies) and runs by opening the file directly in a browser.

## Working in this repo

- To view a page: open the `index.html` file directly in a browser (e.g. `open Session2_BadPrompt/index.html` on macOS). No dev server or build step exists or is needed.
- Each `index.html` is independent — do not factor out "shared" code between the two folders. The point of this repo is the side-by-side comparison, so keep each file self-contained.
- When adding a new comparison (e.g. `Session3_*`), follow the existing pattern: one folder per variant, one self-contained `index.html` inside it, vanilla HTML/CSS/JS (no frameworks, no build tooling).
