# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2026-03-27

### Changed

- Fixed unclosed code fence in `commands/setup.md` (Step 6 summary block)
- Bumped version to `1.0.1` for packaging and documentation cleanup
- Updated `marketplace.json` source format from git URL to GitHub repo format
- Rewrote `README.md` with clear sections for Overview, Architecture & Trust Boundaries, Security, Commands, Skills, Agents
- Tightened security wording — replaced "stored securely" with precise language about local environment variable storage
- Added `.claude/` directory to `.gitignore`

### Added

- `LICENSE` (MIT)
- `CHANGELOG.md`
- `.gitignore`

## [1.0.0] - 2026-03-27

### Added

- **Send SMS** skill — send text messages to single recipients (V2 API) or contact lists/multiple numbers (V1 API), with support for scheduling, link tracking, and delivery callbacks
- **Send MMS** skill — send multimedia messages with images, GIFs, video, or audio attachments (V2 API)
- **Create Contact List** skill — create lists and add contacts for campaign sends (V1 API)
- **Setup Webhook** skill — configure webhooks for delivery status, inbound messages, MMS events, link hits, and opt-outs (V2 API)
- **Setup command** (`/kudosity-sms:setup`) — interactive credential configuration with V1 and V2 API validation
- **Kudosity Assistant agent** — multi-step assistant that coordinates messaging operations with live API documentation support
- **MCP server integration** — connects to `developers.kudosity.com/mcp` for API documentation lookup and endpoint discovery
- `LICENSE` (MIT)
- `CHANGELOG.md`
- `.gitignore`