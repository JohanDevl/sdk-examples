# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a collection of sample projects demonstrating the ServiceNow SDK and Fluent language. Each sample is an independent project in its own directory, showcasing different ServiceNow APIs and capabilities.

## Repository Structure

The repository follows a monorepo pattern with 20+ independent sample projects. Each sample directory contains:

- `src/fluent/` - Fluent interface definitions (`.now.ts` files) that define ServiceNow entities
- `src/server/` or `src/client/` - Implementation code (`.server.js`, `.client.js` files)
- `now.config.json` - ServiceNow scope and configuration
- `package.json` - Individual project dependencies and build scripts
- `README.md` - Sample-specific documentation

Common sample types:
- **Basic samples**: Tables, Records, Lists, ACLs, Application Menus
- **Server-side logic**: Business Rules, Script Includes, REST APIs
- **Client-side logic**: Client Scripts, UI Actions
- **UI samples**: UI Pages with React/Vue/Svelte/SolidJS, Service Portal
- **Testing**: ATF (Automated Test Framework) tests

## Build Commands

### Root Level Commands
```bash
# Install dependencies for all samples in parallel
npm run i

# Install dependencies using npm ci (for clean installs)
npm run ci

# Lint all projects
npm run eslint

# Format check with Prettier
npm run prettier
```

### Individual Sample Commands
Within any sample directory:
```bash
# Build the sample (compiles Fluent definitions)
npm run build

# Deploy/install to ServiceNow instance (some samples)
npm run deploy

# Generate TypeScript types from dependencies (some samples)
npm run types

# Transform code (some samples)
npm run transform
```

## Key Architecture Patterns

### Fluent Language (.now.ts files)
The `.now.ts` files use the ServiceNow Fluent API to declaratively define ServiceNow entities. Key patterns:

- **Naming convention**: Files follow `<entity-name>.now.ts` pattern
- **Table naming**: Tables are prefixed with scope (e.g., `x_helloworld_tableone`)
- **ID references**: Use `Now.ID['identifier']` for entity references
- **Script inclusion**: Use `Now.include('./script-file.js')` to reference implementation files
- **Generated folder**: The `src/fluent/generated/` directory contains auto-generated type definitions from dependencies

### Configuration Files

**now.config.json** - Required in each sample, defines:
- `scope`: Application scope name (e.g., "x_helloworld")
- `scopeId`: Unique identifier for the scope
- `dependencies`: (optional) External table dependencies to import

**Dependencies workflow**:
1. Add table references in `now.config.json` under `dependencies.applications`
2. Run `npm run types` or `now-sdk dependencies` to fetch definitions
3. Generated types appear in `src/fluent/generated/`

### UI Page Samples (React/Vue/Svelte/SolidJS)

UI page samples have a different structure:
- `src/fluent/` - UIPage Fluent definition
- `src/server/` - Server-side TypeScript with `tsconfig.json`
- `src/client/` - Client-side React/Vue/Svelte/SolidJS code
- Use `@servicenow/isomorphic-rollup` for bundling

## SDK Version

All samples use:
- `@servicenow/sdk`: Version 4.0.2
- `@servicenow/glide`: Version 26.0.1
- Node: v20+ (minimum required)
- TypeScript: 5.8.2

## Development Workflow

1. Navigate to a specific sample directory
2. Run `npm install` to install dependencies
3. Modify `.now.ts` files to define entities
4. Modify `.server.js` or `.client.js` files for implementation
5. Run `npm run build` to compile
6. Deploy to ServiceNow instance if needed

## ESLint Configuration

The repository uses:
- `@servicenow/eslint-plugin-sdk-app-plugin` for SDK-specific linting
- `eslint-config-prettier` for Prettier compatibility
- Configuration in root `.eslintrc.json`
