# Property-Based Testing — Opt-In

**Extension**: Property-Based Testing

## Opt-In Prompt

The following question is automatically included in the Requirements Analysis clarifying questions when this extension is loaded. Present this question using the `AskUserQuestion` tool:

**Question**: Should property-based testing (PBT) rules be enforced for this project?

**Options**:
- **Yes** — enforce all PBT rules as blocking constraints (recommended for projects with business logic, data transformations, serialization, or stateful components)
- **Partial** — enforce PBT rules only for pure functions and serialization round-trips (suitable for projects with limited algorithmic complexity)
- **No** — skip all PBT rules (suitable for simple CRUD applications, UI-only projects, or thin integration layers with no significant business logic)
