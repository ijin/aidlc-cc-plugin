# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.6] - 2026-03-05

### Fixed
- Copy-paste error in error-handling.md that duplicated content into wrong section (upstream)

### Added
- Consolidated application-design.md doc instruction in application design phase (upstream)

### Changed
- Removed placeholder .gitkeep files for HIPAA, PCI-DSS, SOC2, and customer-specific compliance directories (upstream cleanup)

### Upstream
- Synced with [aidlc-workflows v0.1.6](https://github.com/aws-samples/aidlc-workflows/releases/tag/v0.1.6)

## [0.1.5] - 2026-02-24

### Added
- Security extensions framework with OWASP Top 10 baseline rules (15 SECURITY rules)
- Extensions loading and enforcement mechanism in core workflow
- Extension applicability questions in requirements analysis (adapted for AskUserQuestion)
- Placeholder directories for HIPAA, PCI-DSS, SOC2, and customer-specific security extensions

### Upstream
- Synced with [aidlc-workflows v0.1.5](https://github.com/aws-samples/aidlc-workflows/releases/tag/v0.1.5)
- Skipped v0.1.3 (bug fix reverted, net zero rule changes) and v0.1.4 (path resolution only, not applicable)

## [0.1.2] - 2026-02-08

### Added
- Frontend components generation steps in code-generation (upstream)
- Automation-friendly code rules with `data-testid` attributes (upstream)
- Frontend components section in functional-design (upstream)
- ⛔ GATE between clarifying questions and requirements generation to prevent premature proceeding (upstream, adapted for chat-based flow)

### Fixed
- "busienss" typo in core-workflow.md (upstream, already fixed in v0.1.0)

### Upstream
- Synced with [aidlc-workflows v0.1.2](https://github.com/aws-samples/aidlc-workflows/releases/tag/v0.1.2)

## [0.1.0] - 2026-02-03

### Added
- Initial marketplace release
- Core AI-DLC methodology with three phases: Inception, Construction, Operations
- Adaptive workflow planning based on project complexity
- Chat-based Q&A interaction model
- Approval gates at major stage transitions
- Progress tracking with `aidlc-docs/aidlc-state.md`
- Complete audit trail with timestamps
- Session continuity (resume capability)
- Brownfield project support with reverse engineering
- Per-unit construction loop with conditional stages
- Adapted from AWS AI-DLC Workflows for Claude Code plugin system

### Notes
- This is a pre-1.0 release - breaking changes may occur
- Based on AWS AI-DLC Workflows reference implementation
