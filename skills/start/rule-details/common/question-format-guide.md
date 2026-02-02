# Question Format Guide

## MANDATORY: Check User's Question Preference First

**CRITICAL**: Before asking any questions, check the user's preferred question style from `aidlc-docs/aidlc-preferences.md`:
- **If preference is "Interactive UI"**: Use the `AskUserQuestion` tool (Section A below)
- **If preference is "Text responses"**: Use text-based questions (Section B below)

---

# SECTION A: Interactive UI Format (AskUserQuestion Tool)

## When User Prefers Interactive UI

### Rule: Use Claude Code's Native Question Tool
**CRITICAL**: You must use the `AskUserQuestion` tool to ask questions. This provides clickable options for easy selection. DO NOT create question files for users to fill out.

### Using AskUserQuestion Tool

#### When to Use
- **Requirements gathering**: Any time you need to clarify user requirements
- **Design decisions**: When multiple valid approaches exist
- **Ambiguity resolution**: When user responses need clarification
- **Planning questions**: During inception or construction planning phases

#### Tool Parameters

```json
{
  "questions": [
    {
      "question": "What is the primary user authentication method?",
      "header": "Auth Method",
      "multiSelect": false,
      "options": [
        {
          "label": "Username and password",
          "description": "Traditional credentials-based authentication"
        },
        {
          "label": "Social media login",
          "description": "OAuth with Google, Facebook, etc."
        },
        {
          "label": "Single Sign-On (SSO)",
          "description": "Enterprise SSO integration"
        },
        {
          "label": "Multi-factor authentication",
          "description": "Enhanced security with 2FA/MFA"
        }
      ]
    }
  ]
}
```

**CRITICAL Guidelines**:
- Ask 1-4 questions per `AskUserQuestion` call (tool limit)
- Each question gets a short `header` (max 12 chars) displayed as a chip
- Each question must have 2-4 `options` (tool enforces this)
- Each option needs a `label` (1-5 words) and `description` (explains the choice)
- Set `multiSelect: false` for single-choice questions (default)
- Set `multiSelect: true` when multiple options can be selected
- The tool automatically provides an "Other" option for custom responses

#### Question Grouping Strategy

**Simple Sets (1-4 questions)**: Use single `AskUserQuestion` call
```
Use AskUserQuestion tool with all questions in one call
```

**Medium Sets (5-8 questions)**: Break into 2 groups by topic
```
First, use AskUserQuestion for core questions (1-4)
Then, use AskUserQuestion for follow-up questions (1-4)
```

**Large Sets (9+ questions)**: Break into 3+ logical phases
```
Phase 1: Use AskUserQuestion for first topic (1-4 questions)
Phase 2: Use AskUserQuestion for second topic (1-4 questions)
Phase 3: Use AskUserQuestion for third topic (1-4 questions)
```

### Complete Example

#### Example 1: Requirements Clarification (3 questions)

```json
{
  "questions": [
    {
      "question": "What is the primary user authentication method?",
      "header": "Auth",
      "multiSelect": false,
      "options": [
        {
          "label": "Username/password",
          "description": "Traditional credentials-based authentication"
        },
        {
          "label": "Social login",
          "description": "OAuth with Google, Facebook, etc."
        },
        {
          "label": "SSO",
          "description": "Enterprise Single Sign-On"
        },
        {
          "label": "MFA",
          "description": "Multi-factor authentication"
        }
      ]
    },
    {
      "question": "Will this be a web or mobile application?",
      "header": "Platform",
      "multiSelect": false,
      "options": [
        {
          "label": "Web application",
          "description": "Browser-based application"
        },
        {
          "label": "Mobile app",
          "description": "iOS/Android native or hybrid"
        },
        {
          "label": "Both",
          "description": "Web and mobile applications"
        }
      ]
    },
    {
      "question": "Is this a new project or existing codebase?",
      "header": "Type",
      "multiSelect": false,
      "options": [
        {
          "label": "Greenfield",
          "description": "New project from scratch"
        },
        {
          "label": "Brownfield",
          "description": "Existing codebase with changes"
        }
      ]
    }
  ]
}
```

#### Example 2: Multi-Select Question

```json
{
  "questions": [
    {
      "question": "Which features do you want to enable?",
      "header": "Features",
      "multiSelect": true,
      "options": [
        {
          "label": "User profiles",
          "description": "User account management and profiles"
        },
        {
          "label": "Notifications",
          "description": "Email and push notifications"
        },
        {
          "label": "Analytics",
          "description": "Usage tracking and reporting"
        },
        {
          "label": "API access",
          "description": "Programmatic API for integrations"
        }
      ]
    }
  ]
}
```

### Processing User Responses

After calling `AskUserQuestion`, the tool returns user answers. Process them as follows:

1. **Parse responses**: Extract user selections from the `answers` object
2. **Validate completeness**: Ensure all questions were answered
3. **Check for "Other"**: If user selected "Other", they provided custom text
4. **Analyze for contradictions**: Check for logically inconsistent answers
5. **Ask follow-ups if needed**: Use another `AskUserQuestion` call for clarifications

### Recording Q&A for Audit Trail

After collecting all answers, create a summary file:

**File**: `aidlc-docs/{phase-name}/questions-summary.md`

**Format**:
```markdown
# [Phase Name] Questions and Answers

**Date**: [ISO timestamp]

## Question 1
**Question**: What is the primary user authentication method?
**Answer**: Single Sign-On (SSO)
**Reasoning**: Enterprise SSO integration

## Question 2
**Question**: Will this be a web or mobile application?
**Answer**: Web application
**Reasoning**: Browser-based application

## Question 3
**Question**: Is this a new project or existing codebase?
**Answer**: Existing codebase (brownfield)
**Reasoning**: Working with existing code

---
**Summary**: User confirmed SSO authentication for a web-based brownfield project.
```

### Error Handling

#### Missing Answers
If any question is not answered (shouldn't happen with the tool, but check):
```
I noticed Question [X] wasn't answered. Let me ask that again.
[Use AskUserQuestion with just that question]
```

#### Ambiguous Custom Responses
If user provided "Other" text that's unclear:
```
Thank you for that context. Let me clarify with a follow-up question.
[Use AskUserQuestion with clarification question]
```

### Contradiction and Ambiguity Detection

**MANDATORY**: After receiving user responses, you MUST check for contradictions and ambiguities.

#### Detecting Contradictions
Look for logically inconsistent answers:
- Scope mismatch: "Bug fix" but "Entire codebase affected"
- Risk mismatch: "Low risk" but "Breaking changes"
- Timeline mismatch: "Quick fix" but "Multiple subsystems"
- Impact mismatch: "Single component" but "Significant architecture changes"

#### Detecting Ambiguities
Look for unclear or borderline responses:
- Answers that could fit multiple classifications
- Responses that lack specificity
- Conflicting indicators across multiple questions

#### Handling Contradictions and Ambiguities

If contradictions or ambiguities detected, ask clarifying questions immediately using `AskUserQuestion`:

**Example**:
```json
{
  "questions": [
    {
      "question": "I noticed you indicated 'bug fix' but also 'entire codebase affected'. Which better describes the scope?",
      "header": "Scope",
      "multiSelect": false,
      "options": [
        {
          "label": "Isolated bug fix",
          "description": "Small, single-component fix"
        },
        {
          "label": "Multi-component fix",
          "description": "Bug fix requiring changes across components"
        },
        {
          "label": "Large refactoring",
          "description": "Major changes disguised as a bug fix"
        }
      ]
    }
  ]
}
```

#### Workflow for Clarifications

1. **Detect**: Analyze all responses for contradictions/ambiguities immediately
2. **Ask**: Use `AskUserQuestion` to present clarifying questions right away
3. **Wait**: Tool handles waiting for user response automatically
4. **Re-validate**: After clarifications, check again for consistency
5. **Record**: Include clarifications in the questions-summary.md file
6. **Proceed**: Only move forward when all contradictions are resolved

### Best Practices

1. **Be Specific**: Questions should be clear and unambiguous
2. **Be Comprehensive**: Cover all necessary information, but respect the 1-4 question limit per call
3. **Be Concise**: Use short headers (max 12 chars) and clear labels (1-5 words)
4. **Be Practical**: Options should be realistic and actionable
5. **Be Descriptive**: Provide helpful descriptions for each option
6. **Be Flexible**: The tool provides "Other" option automatically
7. **Be Thorough**: Always check for contradictions before proceeding

### Phase-Specific Examples

#### Example: Requirements with 2 options

```json
{
  "questions": [
    {
      "question": "Is this a new project or existing codebase?",
      "header": "Type",
      "multiSelect": false,
      "options": [
        {
          "label": "Greenfield",
          "description": "New project from scratch"
        },
        {
          "label": "Brownfield",
          "description": "Existing codebase"
        }
      ]
    }
  ]
}
```

#### Example: Architecture with 3 options

```json
{
  "questions": [
    {
      "question": "What is the deployment target?",
      "header": "Deploy",
      "multiSelect": false,
      "options": [
        {
          "label": "Cloud",
          "description": "AWS, Azure, or GCP"
        },
        {
          "label": "On-premises",
          "description": "Self-hosted servers"
        },
        {
          "label": "Hybrid",
          "description": "Both cloud and on-premises"
        }
      ]
    }
  ]
}
```

#### Example: Pattern with 4 options

```json
{
  "questions": [
    {
      "question": "What architectural pattern should be used?",
      "header": "Architecture",
      "multiSelect": false,
      "options": [
        {
          "label": "Monolithic",
          "description": "Single unified application"
        },
        {
          "label": "Microservices",
          "description": "Distributed service architecture"
        },
        {
          "label": "Serverless",
          "description": "Function-as-a-Service approach"
        },
        {
          "label": "Event-driven",
          "description": "Message-based architecture"
        }
      ]
    }
  ]
}
```

---

# SECTION B: Text-Based Format

## When User Prefers Text Responses

### Rule: Use Conversational Text-Based Questions
If the user prefers text responses, present questions in a clear numbered format that allows them to respond with letter choices or descriptions.

### Text-Based Question Format

Present questions clearly in the conversation with this structure:

```
I need to clarify [X] items to proceed with [stage name]:

**Question 1: [Clear, specific question text]**
A) [First meaningful option]
B) [Second meaningful option]
C) [Additional options as needed]
D) Other (please describe)

**Question 2: [Next question]**
A) [Option 1]
B) [Option 2]
C) Other (please describe)

Please answer each question by providing the letter choice (e.g., "1: A, 2: B") or describe your answer if choosing "Other".
```

**Guidelines**:
- Present questions in a clear, numbered format
- Always include "Other" as the last option for flexibility
- Only include meaningful options - don't make up options to fill slots
- Use as many or as few options as make sense (minimum 2 + Other)
- For simple questions (1-3), present all at once
- For complex question sets (4+), consider grouping into logical sets

### User Response Format
Users will answer directly in conversation:

**Concise Format**:
```
1: C, 2: A, 3: B
```

**Detailed Format**:
```
Question 1: C (SSO)
Question 2: A (Web application)
Question 3: B (Brownfield)
```

**Mixed with "Other"**:
```
1: D - We want to use OAuth 2.0 with Google Workspace
2: A
3: B
```

### Processing User Responses
After receiving answers:
1. Parse and validate all responses
2. Check for contradictions and ambiguities
3. If clarification needed, ask follow-up questions immediately
4. Record Q&A summary in `aidlc-docs/{phase-name}/questions-summary.md` for audit trail

### Contradiction Detection (Same as Section A)
Follow the same contradiction detection and resolution process as described in Section A above.

---

## Summary

**Remember**:
- ✅ Always check user preference in `aidlc-docs/aidlc-preferences.md` first
- ✅ Use `AskUserQuestion` if user prefers "Interactive UI"
- ✅ Use text-based questions if user prefers "Text responses"
- ✅ Validate responses for contradictions immediately
- ✅ Ask follow-up questions using the same format user prefers
- ✅ Record Q&A summary in aidlc-docs/ for audit trail
- ❌ Never create question files for users to edit
- ❌ Never proceed without answers
- ❌ Never proceed with unresolved contradictions
- ❌ Never make assumptions about ambiguous responses
