# Security Baseline — Opt-In

**Extension**: Security Baseline

## Opt-In Prompt

The following question is automatically included in the Requirements Analysis clarifying questions when this extension is loaded. Present this question using the `AskUserQuestion` tool:

**Question**: Should security extension rules be enforced for this project?

**Options**:
- **Yes** — enforce all SECURITY rules as blocking constraints (recommended for production-grade applications)
- **No** — skip all SECURITY rules (suitable for PoCs, prototypes, and experimental projects)
