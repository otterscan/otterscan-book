# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the Otterscan documentation book repository, built using mdbook. Otterscan is a block explorer for EVM chains designed to run locally with an archive node companion (specifically Erigon). The documentation covers installation, configuration, API specifications, and usage guides.

## Development Commands

### Build the book
```shell
mdbook build
```
The compiled book will be available in the `book` subdirectory.

### Local development server
```shell
mdbook serve
```
Starts a local HTTP server at http://localhost:3000/ for live preview during editing.

## Repository Structure

- `src/` - Source markdown files organized by topic
  - `SUMMARY.md` - Table of contents defining book structure
  - `intro/` - Introduction and architecture overview
  - `install/` - Installation guides for different components
  - `config/` - Configuration options and settings
  - `api-docs/` - API specifications (ots and ots2 namespaces)
  - `contract-verification/` - Sourcify integration guides
  - `cookbook/` - Specific setup examples and recipes
- `book.toml` - mdbook configuration
- `book/` - Generated HTML output (auto-generated, don't edit)
- `theme/` - Custom theme files

## Architecture Context

The documentation covers a three-component architecture:
1. **Otterscan UI** - React single-page application (separate repo)
2. **Otterscan API Specification** - API definitions documented here
3. **Otterscan API Implementation** - Reference implementation in Erigon

When editing content, consider that readers may be setting up either development or production environments, with various blockchain networks and optional components like beacon chain support.

## Content Guidelines

- Follow existing markdown structure and formatting
- Update `SUMMARY.md` when adding new pages
- Place images in appropriate `images/` subdirectories
- Use relative links between documentation pages
- Maintain consistent heading hierarchy within sections