# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Homebridge plugin for SimpliSafe 3 security systems with night mode support. It's a fork of the original homebridge-simplisafe3 plugin that integrates SimpliSafe devices with Apple HomeKit through Homebridge.

## Development Commands

- **Build**: `npm run build` - Transpiles ES6+ code from `src/` to `dist/` using Babel, copies necessary files
- **Start Development**: `npm run start` - Runs the plugin in development mode with auto-restart via nodemon
- **Deploy**: `npm run deploy` - Publishes the built package from `dist/` directory to npm
- **Release**: `./scripts/release.sh` - Full release script that builds, deploys, and tags versions
- **Lint**: Uses ESLint with Babel parser (see eslintConfig in package.json)

## Architecture

### Core Components

**Main Platform (`src/index.js`)**:
- `SS3Platform` class orchestrates device discovery and accessory management
- Handles authentication via `SimpliSafe3AuthenticationManager`
- Manages device lifecycle and Homebridge integration

**SimpliSafe API Client (`src/simplisafe.js`)**:
- `SimpliSafe3` class extends EventEmitter for real-time events
- Handles rate limiting, WebSocket connections, and API requests
- Manages authentication tokens and subscription data

**Device Accessories (`src/accessories/`)**:
- Each device type (alarm, sensors, cameras, locks) has its own accessory class
- All inherit from `ss3Accessory.js` base class
- Handle HomeKit characteristic updates and SimpliSafe API integration

**Authentication (`src/lib/authManager.js`)**:
- Manages OAuth authentication flow with SimpliSafe
- Handles token refresh and credential storage

### Key Patterns

- **Event-driven architecture**: Real-time updates via WebSocket events
- **Rate limiting**: Built-in protection against API rate limits with exponential backoff
- **Caching**: Subscription and sensor data cached for performance
- **Error handling**: Comprehensive error suppression and recovery mechanisms

### Device Support

The plugin supports alarms, various sensors (entry, motion, smoke, CO, water, freeze), smart locks, and cameras (SimpliCam, Video Doorbell Pro). Each device type maps to appropriate HomeKit services and characteristics.

### Configuration

OAuth authentication is required - users authenticate via browser and provide the resulting URL. The plugin supports multiple SimpliSafe accounts via `subscriptionId` parameter.

## Testing

Currently uses placeholder test command (`echo NO TESTS`). The project would benefit from proper test implementation.

## Release Process

1. Update version in `CHANGELOG.md`
2. Run `./scripts/release.sh` which handles version updates, building, npm deployment, and git tagging
3. Must be on `master` branch for releases