# AI-DLC Plugin for Claude Code

A structured, adaptive software development methodology guided by AI.

## Installation

First, add the marketplace to your Claude Code (one-time setup):

```bash
/plugin marketplace add ijin/aidlc-cc-plugin
```

Then install the plugin:

```bash
/plugin install aidlc@aidlc-cc-plugin
```

## Quick Start

Start a new AI-DLC workflow:

```bash
/aidlc:start Develop a recommendation engine for cross-selling products
```

Or resume an existing session:

```bash
/aidlc:start
```

Claude will automatically detect your previous session (via `aidlc-docs/aidlc-state.md`) and offer to resume where you left off.

## How It Works

AI-DLC guides you through a three-phase adaptive workflow:

```
┌─────────────────────────────────────────────────────────────┐
│                      USER REQUEST                           │
│         "Develop a recommendation engine..."                │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                  INCEPTION PHASE                            │
│           Planning & Architecture                           │
├─────────────────────────────────────────────────────────────┤
│  • Workspace Detection (greenfield/brownfield)              │
│  • Reverse Engineering (brownfield only)                    │
│  • Requirements Analysis (adaptive depth)                   │
│  • User Stories (conditional)                               │
│  • Workflow Planning (adaptive - user approval)             │
│  • Application Design (conditional)                         │
│  • Units Generation (conditional)                           │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                 CONSTRUCTION PHASE                          │
│            Design & Implementation                          │
├─────────────────────────────────────────────────────────────┤
│  Per-Unit Loop:                                             │
│    • Functional Design (conditional)                        │
│    • NFR Requirements (conditional)                         │
│    • NFR Design (conditional)                               │
│    • Infrastructure Design (conditional)                    │
│    • Code Generation (always - with plan approval)          │
│                                                             │
│  After All Units:                                           │
│    • Build and Test (always)                                │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│                  OPERATIONS PHASE                           │
│               (Placeholder for Future)                      │
└─────────────────────────────────────────────────────────────┘
```

### Key Principles

**Adaptive Workflow** — Stages execute only when they add value. Simple changes skip unnecessary planning. Complex projects get full treatment.

**User Control** — Approve execution plans before work begins. Request changes at any approval gate. Resume anytime after closing your session.

**Complete Audit Trail** — All decisions, user inputs, and stage transitions logged with timestamps in `aidlc-docs/audit.md`.

**Chat-Based Interaction** — Answer questions directly in conversation. No file editing required.

## Managing Artifacts

AI-DLC creates comprehensive documentation in the `aidlc-docs/` directory:

```
<workspace-root>/
├── [your application code]
└── aidlc-docs/
    ├── aidlc-state.md           # Progress tracking with checkboxes
    ├── audit.md                 # Complete audit trail with timestamps
    ├── inception/
    │   ├── workspace-analysis.md
    │   ├── reverse-engineering/  # (brownfield only)
    │   ├── requirements/
    │   ├── user-stories/         # (if executed)
    │   ├── application-design/   # (if executed)
    │   └── plans/
    ├── construction/
    │   ├── unit-01/
    │   │   ├── functional-design/
    │   │   ├── nfr-requirements/
    │   │   ├── nfr-design/
    │   │   └── infrastructure-design/
    │   └── build-and-test/
    └── operations/               # (placeholder)
```

### Should you commit aidlc-docs/?

**Option 1: Commit Everything (Recommended for team projects)**
- Full audit trail and design documentation for the team
- Enables review of AI decisions
- Provides onboarding context for new team members

```bash
git add aidlc-docs/
git commit -m "Add AI-DLC artifacts"
```

**Option 2: Gitignore (Recommended for personal projects)**
- Treat as local working notes
- Reduces repository size
- Regenerate on demand

```bash
echo "aidlc-docs/" >> .gitignore
```

**Option 3: Selective Commit**
- Commit key design documents (`application-design`, `functional-design`)
- Ignore temporary artifacts (`workspace-analysis.md`, `audit.md`)

```bash
# Add to .gitignore
aidlc-docs/audit.md
aidlc-docs/aidlc-state.md
aidlc-docs/inception/workspace-analysis.md
```

## Examples

### Greenfield Project
```bash
/aidlc:start Build a REST API for managing customer subscriptions with Stripe integration
```

AI-DLC will guide you through requirements, design high-level architecture, break work into units, and generate code.

### Brownfield Enhancement
```bash
/aidlc:start Add user authentication with OAuth2 to the existing API
```

AI-DLC will analyze your codebase first, then adapt the workflow to integrate with your existing architecture.

### Simple Bug Fix
```bash
/aidlc:start Fix the race condition in the payment processing queue
```

AI-DLC will skip unnecessary stages and focus on the specific issue.

## Acknowledgments

This plugin is adapted from the [AWS AI-DLC Workflows](https://github.com/aws-samples/aidlc-workflows) reference implementation, originally designed for Amazon Q Developer / Kiro CLI. It has been adapted for Claude Code's plugin system with chat-based interaction patterns.

Original methodology developed by AWS Samples team.

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-improvement`)
3. Make your changes
4. Test thoroughly with `claude --plugin-dir .`
5. Submit a pull request

No DCO or formal process required — just good code and clear commit messages.

## License

This project is licensed under MIT-0 - see the [LICENSE](LICENSE) file for details.

MIT-0 is a permissive license that allows use without attribution requirements.
