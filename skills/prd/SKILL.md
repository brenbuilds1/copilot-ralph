---
name: prd-generator
description: "Generate a Product Requirements Document (prd.json) for the Ralph autonomous agent loop. Use when planning a feature, starting a new project, or preparing work for Ralph to implement. Triggers on: create a prd, write prd for, plan this feature, generate user stories, set up ralph for, spec out."
---

# PRD Generator

Create detailed Product Requirements Documents that are clear, actionable, and suitable for autonomous implementation by Ralph.

---

## The Job

1. Receive a feature description from the user
2. Ask 3-5 essential clarifying questions (with lettered options)
3. Generate a structured PRD based on answers
4. Save to `prd.json` in project root

**Important:** Do NOT start implementing. Just create the PRD.

---

## Step 1: Clarifying Questions

Ask only critical questions where the initial prompt is ambiguous. Focus on:

- **Problem/Goal:** What problem does this solve? Why build this?
- **Core Functionality:** What are the key user actions?
- **Scope/Boundaries:** What should it NOT do?
- **Technology:** What stack/framework to use?
- **Success Criteria:** How do we know it's done?

### Format Questions Like This:

```
1. What is the primary goal of this feature?
   A. Improve user onboarding experience
   B. Increase user retention
   C. Reduce support burden
   D. Other: [please specify]

2. Who is the target user?
   A. New users only
   B. Existing users only
   C. All users
   D. Admin users only

3. What is the scope?
   A. Minimal viable version (MVP)
   B. Full-featured implementation
   C. Just the backend/API
   D. Just the UI/frontend

4. What technology stack?
   A. React + Node.js
   B. Next.js
   C. Plain HTML/CSS/JS
   D. Other: [please specify]

5. What's explicitly OUT of scope?
   A. Authentication/login
   B. Database persistence
   C. Mobile responsiveness
   D. None of the above - include everything
```

This lets users respond with "1A, 2C, 3A, 4B, 5A" for quick iteration.

---

## Step 2: Break Down Into User Stories

After gathering answers, break the feature into small, focused user stories.

### Story Size Rule

Each story must be **small enough to complete in one Ralph iteration** (one agent context window, ~15-30 minutes of focused work).

**Good story sizes:**
- Add a single component
- Create one API endpoint
- Add form validation
- Create a database migration
- Wire up frontend to API

**Too large (must split):**
- "Build the dashboard" → Split into: layout, sidebar, header, each widget
- "Add authentication" → Split into: login form, API endpoint, session handling, protected routes
- "Create the API" → Split into: one endpoint per story

### Story Format

Each story needs:
- **id:** Unique identifier (US-001, US-002, etc.)
- **title:** Short action-oriented name
- **priority:** Order of implementation (1 = first)
- **description:** Context for implementation - what and why
- **acceptanceCriteria:** Verifiable checklist of "done"

### Writing Good Acceptance Criteria

Criteria must be **specific and verifiable**, not vague.

**Bad (too vague):**
- "Works correctly"
- "Handles errors"
- "Is responsive"
- "Looks good"

**Good (specific and testable):**
- "Form shows red border and error message when email format is invalid"
- "API returns 400 status with `{ error: 'Invalid email' }` for malformed input"
- "Button is disabled and shows spinner while request is in flight"
- "Cards stack vertically on screens < 768px wide"

### Standard Criteria to Include

For every story, include relevant items from:
- "Typecheck passes" (if using TypeScript)
- "Lint passes"
- "No console errors in browser"

For UI stories, always include:
- "Visually verified in browser"
- Specific responsive behavior if applicable

For API stories, always include:
- Success response format
- Error response format
- Edge cases (duplicate, not found, etc.)

---

## Step 3: Define Non-Goals

Explicitly state what this PRD will NOT include. This prevents scope creep and sets clear boundaries.

Examples:
- "No user authentication - assumes public access"
- "No email notifications"
- "No mobile app - web only"
- "No analytics tracking"

---

## Step 4: Order by Dependencies

Arrange stories so foundations come first:

1. **Priority 1-2:** Setup, scaffolding, database schema
2. **Priority 3-5:** Core functionality, main features
3. **Priority 6+:** Enhancements, polish, nice-to-haves

A story cannot depend on a higher-numbered story.

---

## Writing for Junior Developers and AI Agents

The PRD reader may be a junior developer or AI agent (Ralph). Therefore:

- Be explicit and unambiguous
- Avoid jargon or explain technical terms
- Provide enough detail to understand purpose and implementation approach
- Use concrete examples where helpful
- Each acceptance criterion should be independently verifiable

---

## Output Format

Save as `prd.json` in the project root:

```json
{
  "projectName": "Feature Name",
  "branchName": "ralph/feature-name",
  "description": "Brief description of the feature and problem it solves",
  "userStories": [
    {
      "id": "US-001",
      "title": "Short action-oriented title",
      "priority": 1,
      "description": "Detailed context: what to build and why. Include relevant technical details.",
      "acceptanceCriteria": [
        "Specific verifiable criterion",
        "Another specific criterion",
        "Typecheck passes"
      ],
      "passes": false
    }
  ]
}
```

**Fields:**
- `projectName`: Human-readable feature name
- `branchName`: Git branch (convention: `ralph/kebab-case-name`)
- `description`: Overview of the feature and problem it solves
- `userStories`: Array of stories, ordered by priority
- `passes`: Always `false` initially (Ralph sets to `true` when complete)

---

## Example PRD

```json
{
  "projectName": "Task Priority System",
  "branchName": "ralph/task-priority",
  "description": "Add priority levels to tasks so users can focus on what matters most. Tasks can be marked as high, medium, or low priority with visual indicators and filtering.",
  "userStories": [
    {
      "id": "US-001",
      "title": "Add priority field to database schema",
      "priority": 1,
      "description": "Add a priority column to the tasks table to persist priority across sessions. Priority should be an enum of 'high', 'medium', or 'low' with 'medium' as the default.",
      "acceptanceCriteria": [
        "Migration adds priority column to tasks table",
        "Column type is enum: 'high' | 'medium' | 'low'",
        "Default value is 'medium'",
        "Migration runs without errors",
        "Existing tasks get 'medium' priority"
      ],
      "passes": false
    },
    {
      "id": "US-002",
      "title": "Display priority badge on task cards",
      "priority": 2,
      "description": "Show a colored badge on each task card indicating its priority level. Users should see priority at a glance without hovering or clicking.",
      "acceptanceCriteria": [
        "Each task card shows a priority badge",
        "High priority: red badge with 'High' text",
        "Medium priority: yellow badge with 'Medium' text",
        "Low priority: gray badge with 'Low' text",
        "Badge is visible without interaction",
        "Typecheck passes",
        "Visually verified in browser"
      ],
      "passes": false
    },
    {
      "id": "US-003",
      "title": "Add priority selector to task edit form",
      "priority": 3,
      "description": "Allow users to change a task's priority when editing it. The selector should show the current priority and save changes immediately.",
      "acceptanceCriteria": [
        "Priority dropdown appears in task edit modal",
        "Dropdown shows all three options: High, Medium, Low",
        "Current priority is pre-selected",
        "Selection change saves to database immediately",
        "UI updates to reflect new priority without page reload",
        "Typecheck passes",
        "Visually verified in browser"
      ],
      "passes": false
    },
    {
      "id": "US-004",
      "title": "Add priority filter to task list",
      "priority": 4,
      "description": "Add a filter dropdown to the task list header so users can view only tasks of a specific priority level.",
      "acceptanceCriteria": [
        "Filter dropdown in task list header",
        "Options: All, High, Medium, Low",
        "Selecting a filter shows only matching tasks",
        "Filter state persists in URL query params",
        "Shows 'No tasks found' when filter has no matches",
        "Typecheck passes",
        "Visually verified in browser"
      ],
      "passes": false
    }
  ]
}
```

---

## After Generation

Tell the user:

```
PRD saved to prd.json

To start Ralph:
  ./ralph.sh

To check story status:
  cat prd.json | jq '.userStories[] | {id, title, passes}'
```

---

## Checklist

Before saving the PRD:

- [ ] Asked 3-5 clarifying questions with lettered options
- [ ] Incorporated user's answers into the PRD
- [ ] Each user story is small enough for one iteration
- [ ] Stories are ordered by dependency (foundations first)
- [ ] Acceptance criteria are specific and verifiable (not vague)
- [ ] Non-goals/scope boundaries are clear from description
- [ ] Branch name follows `ralph/feature-name` convention
- [ ] All stories have `"passes": false`
- [ ] Saved to `prd.json`
