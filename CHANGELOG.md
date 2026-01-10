# Changelog

All notable changes to the Fotios Claude System will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.27.3] - 2026-01-10

### Fixed
- **Claude Assistant**: Auto-configure `.claude.json` to skip all interactive prompts
  - Automatically sets `hasCompletedOnboarding: true` (skips theme selection)
  - Automatically sets `bypassPermissionsModeAccepted: true` (skips warning)
  - Config patched on status check and before Claude starts
  - Works on fresh installs without manual configuration

## [2.27.2] - 2026-01-10

### Fixed
- **Navigation Header**: Standardized header navigation across all pages
  - Consistent menu order: Dashboard, Projects, Tickets, Console, History
  - Logout moved to fixed position on the right
  - Active page highlighting on all pages

## [2.27.1] - 2026-01-10

### Fixed
- **Claude Assistant**: Fixed interactive mode asking for activation/theme/trust
  - Added `--dangerously-skip-permissions` flag to bypass permission prompts
  - Now starts directly without setup wizard
  - Uses inherited environment from web process (`os.environ.copy()`)

## [2.27.0] - 2026-01-09

### Added
- **Upgrade Script** (`upgrade.sh`): New automated upgrade system
  - `--dry-run` mode to preview changes without applying them
  - `-y` flag for auto-confirm (non-interactive mode)
  - Automatic backup before upgrade
  - Database migrations support with version tracking
  - Service stop/start management
  - Changelog display after upgrade
- **Database Migrations**: New `migrations/` folder for schema updates
  - Version-tracked migrations via `schema_migrations` table
  - SQL migration files with naming convention: `VERSION_description.sql`
  - Example migration template included

### Fixed
- **Password Change Script**: Fixed admin panel password change not working
  - Changed from MySQL root to application user credentials
  - Now reads from `/etc/fotios-claude/system.conf` (world-readable)
  - Properly generates and stores bcrypt password hashes

### Documentation
- Added upgrade instructions to README and INSTALL
- Migration README with examples and best practices

## [2.26.17] - 2026-01-09

### Added
- **Claude Activation via Web Panel**: New web-based terminal for activating Claude Code CLI
  - Supports both Anthropic Subscription (OAuth) and API Key authentication
  - Integrated activation modal in dashboard header
  - Real-time PTY terminal using xterm.js
  - Status indicator shows activation state (green=active, orange blinking=inactive)
- **Claude Assistant Page**: Full-page interactive Claude terminal at `/claude-assistant`
  - Direct access to Claude CLI through browser
  - Start/Stop session controls
  - Real-time terminal output
- **Tickets List Page**: New `/tickets` route with full ticket management
  - Filter by status (All, Open, In Progress, Awaiting Input, Done, Failed)
  - Search across ticket numbers, titles, and project names
  - Shows created date, updated date, project, priority, and token usage
  - Keyboard shortcut: Ctrl+K or / to focus search
- **Load Test Report**: Comprehensive system performance documentation
  - 10-ticket parallel processing test results
  - Memory, CPU, and resource usage metrics
  - Recommendations for production deployment

### Changed
- **Aurora Theme**: Updated UI across all pages with consistent dark theme
  - Login page: New aurora background with animated gradient blobs
  - Editor page: Matching glass-morphism design
  - Tickets list: Modern card-based layout with status colors
  - Dashboard: Added Claude activation buttons in header
- **setup.sh**: Now automatically installs Claude Code CLI during system setup
  - Runs `curl -fsSL https://claude.ai/install.sh | bash` for claude user
  - Adds `~/.local/bin` to PATH
  - Updated info message to reference web-based activation

### Fixed
- **Editor "Discard Changes" Bug**: Fixed false positive when closing unmodified files
  - Now compares content with original instead of tracking any change event
  - Properly handles undo (Ctrl+Z) returning to unmodified state

### Technical
- Added PTY-based terminal sessions for Claude activation
- Clean environment isolation for child processes (prevents credential leakage)
- New API endpoints:
  - `/api/claude/status` - Check activation status
  - `/api/claude/activate/*` - Terminal session management
  - `/api/claude/apikey` - Save API key
  - `/api/claude/chat/*` - Claude Assistant sessions

## [2.26.16] - 2026-01-08

### Previous Release
- See GitHub releases for full history

---

For more information, see the [README](README.md).
