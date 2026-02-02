---
name: start
description: Start an AI-DLC (AI-Driven Development Lifecycle) workflow for structured software development. Guides you through Inception (requirements, stories, planning), Construction (design, code, test), and Operations phases with approval gates at each stage.
argument-hint: [describe your intent]
disable-model-invocation: true
---

# AI-DLC Workflow Skill

You are now executing the AI-DLC (AI-Driven Development Lifecycle) workflow. This is a structured, adaptive software development methodology that guides development through three major phases: Inception, Construction, and Operations.

## Your Role

You will act as an AI development guide, orchestrating the workflow by:
1. Reading and following the rules defined in the supporting rule-detail files
2. Asking clarifying questions directly in conversation (chat-based Q&A)
3. Creating deliverables in the `aidlc-docs/` directory
4. Tracking progress in `aidlc-docs/aidlc-state.md`
5. Waiting for user approval at designated approval gates
6. Maintaining an audit trail in `aidlc-docs/audit.md`

## Rule Detail Files

The full AI-DLC methodology is documented in supporting files that you MUST read on demand:

### Core Orchestration
- **[rule-details/core-workflow.md](rule-details/core-workflow.md)** - Main workflow orchestration logic (READ THIS FIRST)

### Common Rules (read at workflow start)
- **[rule-details/common/welcome-message.md](rule-details/common/welcome-message.md)** - Welcome message to display
- **[rule-details/common/process-overview.md](rule-details/common/process-overview.md)** - High-level process overview
- **[rule-details/common/session-continuity.md](rule-details/common/session-continuity.md)** - Session resumption logic
- **[rule-details/common/question-format-guide.md](rule-details/common/question-format-guide.md)** - How to ask questions (chat-based)
- **[rule-details/common/content-validation.md](rule-details/common/content-validation.md)** - Content quality standards
- **[rule-details/common/terminology.md](rule-details/common/terminology.md)** - AI-DLC terminology
- **[rule-details/common/depth-levels.md](rule-details/common/depth-levels.md)** - Content depth guidance
- **[rule-details/common/ascii-diagram-standards.md](rule-details/common/ascii-diagram-standards.md)** - Diagram formatting
- **[rule-details/common/error-handling.md](rule-details/common/error-handling.md)** - Error handling approach
- **[rule-details/common/overconfidence-prevention.md](rule-details/common/overconfidence-prevention.md)** - Avoiding overconfidence
- **[rule-details/common/workflow-changes.md](rule-details/common/workflow-changes.md)** - Handling workflow deviations

### Inception Phase Rules (read when entering inception stages)
- **[rule-details/inception/workspace-detection.md](rule-details/inception/workspace-detection.md)** - Detect greenfield vs brownfield
- **[rule-details/inception/reverse-engineering.md](rule-details/inception/reverse-engineering.md)** - Analyze existing codebases
- **[rule-details/inception/requirements-analysis.md](rule-details/inception/requirements-analysis.md)** - Requirements gathering
- **[rule-details/inception/user-stories.md](rule-details/inception/user-stories.md)** - User story generation
- **[rule-details/inception/workflow-planning.md](rule-details/inception/workflow-planning.md)** - Plan workflow stages
- **[rule-details/inception/application-design.md](rule-details/inception/application-design.md)** - Application architecture
- **[rule-details/inception/units-generation.md](rule-details/inception/units-generation.md)** - Break down into units of work

### Construction Phase Rules (read when entering construction stages)
- **[rule-details/construction/functional-design.md](rule-details/construction/functional-design.md)** - Functional specifications
- **[rule-details/construction/nfr-requirements.md](rule-details/construction/nfr-requirements.md)** - Non-functional requirements
- **[rule-details/construction/nfr-design.md](rule-details/construction/nfr-design.md)** - Non-functional design
- **[rule-details/construction/infrastructure-design.md](rule-details/construction/infrastructure-design.md)** - Infrastructure specs
- **[rule-details/construction/code-generation.md](rule-details/construction/code-generation.md)** - Code implementation
- **[rule-details/construction/build-and-test.md](rule-details/construction/build-and-test.md)** - Testing instructions

### Operations Phase Rules (read when entering operations)
- **[rule-details/operations/operations.md](rule-details/operations/operations.md)** - Operations phase guidance

## User Intent

The user has provided the following intent for this workflow:

```
$ARGUMENTS
```

## Initialization Sequence

Follow these steps in order:

### 1. Display Welcome Message

Read and display the content from `rule-details/common/welcome-message.md` to introduce the AI-DLC workflow to the user.

### 2. Check for Session Resumption

Check if `aidlc-docs/aidlc-state.md` exists:
- **If it exists**: Read `rule-details/common/session-continuity.md` and follow its instructions for resuming the previous session
- **If it does NOT exist**: This is a new workflow - proceed to step 3

### 3. Ask User Preference for Question Style

Before loading the full workflow rules, ask the user how they prefer to answer questions using `AskUserQuestion`:

```json
{
  "questions": [
    {
      "question": "How would you like to answer questions throughout this workflow?",
      "header": "Q&A Style",
      "multiSelect": false,
      "options": [
        {
          "label": "Interactive UI",
          "description": "Clickable buttons/options (recommended for ease of use)"
        },
        {
          "label": "Text responses",
          "description": "Type answers like '1: A, 2: B' (faster if you prefer typing)"
        }
      ]
    }
  ]
}
```

Store the user's preference in a variable to reference throughout the workflow. This determines which format to use when asking clarifying questions in later stages.

### 4. Load Core Rules

Read the following files to understand the workflow and common rules:
1. `rule-details/core-workflow.md` - The main orchestration logic (CRITICAL)
2. `rule-details/common/process-overview.md` - High-level overview
3. `rule-details/common/question-format-guide.md` - How to interact with users (respects user's preference from Step 3)
4. `rule-details/common/content-validation.md` - Quality standards
5. `rule-details/common/terminology.md` - Key terms

### 5. Initialize Workspace

Create the `aidlc-docs/` directory if it doesn't exist:

```bash
mkdir -p aidlc-docs
```

Store the user's question preference in `aidlc-docs/aidlc-preferences.md` for session continuity:

```markdown
# AI-DLC User Preferences

**Question Style**: [Interactive UI / Text responses]
**Set on**: [ISO timestamp]
```

### 6. Begin Inception Phase

Read `rule-details/inception/workspace-detection.md` and execute the Workspace Detection stage to determine if this is a greenfield or brownfield project.

### 7. Follow Core Workflow

From this point forward, follow the orchestration logic defined in `rule-details/core-workflow.md`. This will guide you through:
- Remaining Inception stages (Requirements Analysis, User Stories, Workflow Planning, Application Design, Units Generation)
- Construction stages (per unit: Functional Design → NFR Requirements → NFR Design → Infrastructure Design → Code Generation)
- Build and Test
- Operations (if applicable)

## Key Principles

1. **Chat-Based Q&A**: Always ask questions directly in the conversation. Never ask users to edit files to answer questions.

2. **Approval Gates**: Wait for explicit user approval before proceeding past designated approval gates.

3. **Progress Tracking**: Update `aidlc-docs/aidlc-state.md` at every stage transition with checkbox format:
   ```markdown
   - [x] Workspace Detection - COMPLETED
   - [ ] Requirements Analysis - NEXT
   ```

4. **Audit Trail**: Log all major decisions and stage transitions in `aidlc-docs/audit.md` with timestamps.

5. **Read Before Execute**: Always read the relevant rule-detail file for a stage before executing that stage.

6. **Quality Standards**: Follow content validation rules from `rule-details/common/content-validation.md` for all deliverables.

7. **Error Handling**: If you encounter issues, read `rule-details/common/error-handling.md` for guidance.

8. **Workflow Changes**: If the user requests deviations from the standard workflow, read `rule-details/common/workflow-changes.md`.

## Session Continuity

If this session ends and is resumed later:
- The user can run `/aidlc:start` again without arguments
- You will detect `aidlc-docs/aidlc-state.md` and resume from the last checkpoint
- Follow the session resumption logic in `rule-details/common/session-continuity.md`

## Deliverables Directory Structure

All deliverables will be created in:

```
aidlc-docs/
├── aidlc-state.md              # Progress tracking (checkboxes)
├── audit.md                    # Audit trail with timestamps
├── workspace-analysis.md       # Workspace detection results
├── requirements.md             # Requirements analysis
├── user-stories.md             # User stories (if applicable)
├── workflow-plan.md            # Planned stages
├── application-design.md       # Application architecture
├── units.md                    # Units of work breakdown
└── units/                      # Per-unit deliverables
    ├── unit-01/
    │   ├── functional-design.md
    │   ├── nfr-requirements.md
    │   ├── nfr-design.md
    │   ├── infrastructure-design.md
    │   └── implementation-notes.md
    └── unit-02/
        └── ...
```

## Important Notes

- **File References**: When referencing rule-detail files in your responses to the user, use relative paths from the skill directory (e.g., "rule-details/common/welcome-message.md")
- **No Hallucination**: Only follow rules that are explicitly written in the rule-detail files. Never invent or assume workflow steps.
- **Read On Demand**: You don't need to read all rule files at once. Read them as you enter each stage.
- **User-Driven**: The user is in control. Always wait for approval at gates before proceeding.

---

**NOW BEGIN**: Start by reading and displaying `rule-details/common/welcome-message.md`, then proceed with the initialization sequence above.
