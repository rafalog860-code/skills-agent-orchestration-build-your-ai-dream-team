# Project Pulse Dashboard Implementation Plan

## Goal

Build Mona's lightweight, static Project Pulse dashboard so contributors can quickly see active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The dashboard should have a polished, responsive layout with accessible project cards, clear status badges, and readable spacing.

The deliverable consists of `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`. The page must load the project data and the VS Code launch configuration must serve the `app/` directory and open `index.html`, rather than showing a directory listing.

## Agent responsibilities

- **Orchestrator:** Ask the Planner for this plan before implementation, assign explicit file ownership and phases, coordinate dependencies and parallel work, and verify the integrated dashboard against the acceptance criteria.
- **Planner:** Research the brief and repository, define the data contract, file assignments, dependencies, edge cases, and validation expectations. The Planner does not implement files.
- **Designer:** Guide the information hierarchy, responsive layout, visual system, accessible contrast and semantics, project-card structure, status badges, and priority treatment. Provide design direction for `app/styles.css` and the relevant structure in `app/index.html`; do not edit the Coder-owned files unless explicitly assigned.
- **Coder:** Own implementation of `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`. Integrate the Designer's direction, load and render the project data, configure a reliable local preview, and validate the result.

## Phases and file assignments

### Phase 1 — Confirm requirements and design direction

- **Lead:** Planner; Designer contributes UI/UX guidance.
- **Files:** Read `.github/project-pulse-brief.md` and the existing repository setup. No application files are edited in this phase.
- **Work:** Confirm dashboard content, required project fields, responsive and accessibility expectations, and a visual direction before implementation.
- **Dependency:** Complete before the Coder finalizes the page structure and styling.

### Phase 2 — Define project data and build the dashboard

- **Lead:** Coder, using the Designer's direction.
- **File assignments:**
  - `app/project-data.json` — Coder: create a top-level `projects` array. Each project must include `name`, `owner`, `status`, `recentActivity`, and `priority`; provide enough realistic sample entries to demonstrate active projects and different statuses and priorities. Include a short contributor-friendly summary where needed to satisfy the brief.
  - `app/index.html` — Coder: create the accessible page structure, link the stylesheet, load and render the JSON data, and provide clear feedback if the data cannot be loaded.
  - `app/styles.css` — Coder: implement the Designer's responsive visual direction, including `.dashboard` and `.project-card` hooks, readable spacing, status badges, and clear priority treatment.
- **Dependencies:** The data shape must be agreed before the page renders it. The HTML and CSS are coupled through their class names and semantic structure, so the Coder should implement and validate them together. The Designer provides guidance before styling is finalized.

### Phase 3 — Configure and verify the local launch

- **Lead:** Coder; Orchestrator verifies.
- **File assignment:** `.vscode/launch.json` — Coder: add the **Run Project Pulse Dashboard** configuration. Serve from `${workspaceFolder}/app` and open `index.html` so the page can fetch its JSON data and the browser does not show a directory listing. Keep the configuration valid JSON and use available repository tooling; identify any required browser/debugger or server prerequisite rather than assuming it is installed.
- **Dependencies:** Depends on the app paths and data-loading approach from Phase 2. If starting the server requires an additional VS Code task or other file, the Orchestrator must assign that scope explicitly before it is added.

### Phase 4 — Integrated validation

- **Lead:** Orchestrator; Coder reports implementation checks.
- **Files:** Validate all four deliverables together; make corrections only within their assigned ownership.
- **Dependencies:** Run after Phases 2 and 3.

## Dependencies and parallel-work decisions

- The brief and Designer's visual/accessibility guidance inform the HTML structure and CSS. Final styling should follow the agreed page structure.
- The project-data schema is an input to HTML rendering. Define the required fields before implementing the renderer.
- HTML and CSS should be treated as one integrated implementation because their structure and styling hooks must match.
- Data content can be prepared in parallel with the Designer's visual exploration because those tasks do not edit the same files. The Coder should not finalize the HTML renderer until the data contract is agreed, and should not finalize CSS until the Designer's direction and HTML hooks are clear.
- `launch.json` depends on the completed app location and its data-loading approach; implement it after the app paths are known.
- Integrated validation is sequential and follows all implementation work. The Orchestrator should not delegate simultaneous edits to overlapping file scopes.

## Edge cases and implementation notes

- Handle missing, invalid, or empty project data without leaving a blank or misleading dashboard; show a clear user-facing message.
- Keep status and priority labels understandable in text, not color alone, and ensure the layout remains usable at narrow viewport widths.
- Ensure each project card presents the required fields with readable labels and a short summary.
- Because the page loads a JSON file, preview it through a local HTTP server; opening the HTML directly as a `file://` URL may prevent data loading in the browser.
- Avoid introducing dependencies or extra files unless the launch mechanism requires them and the Orchestrator approves their ownership.

## Validation expectations

1. Parse `app/project-data.json` as JSON and verify it has a top-level `projects` array whose entries contain `name`, `owner`, `status`, `recentActivity`, and `priority`.
2. Verify `app/index.html` links `app/styles.css` by the correct relative path and loads `project-data.json`; confirm it renders project cards and handles load errors.
3. Check that the markup includes accessible page structure and visible text for project status and priority, and that the CSS includes the `.dashboard` and `.project-card` hooks and responsive behavior.
4. Parse `.vscode/launch.json` as strict JSON and verify the **Run Project Pulse Dashboard** configuration serves from the `app/` directory and opens `index.html` rather than a directory URL.
5. Launch the configuration in VS Code (with its required browser/debugger available) and confirm the dashboard loads with project data and remains readable at desktop and narrow viewport widths.
6. Report any validation that cannot run in the current environment and the prerequisite needed; do not treat an unverified launch as successful.

## Open questions

- Which launch mechanism and browser/debugger are available in the learner's VS Code environment? Confirm during implementation and keep any required setup explicit.
- The brief requires a contributor-friendly summary but lists five required data fields without naming a summary field. The Coder should include a `summary` field if needed to display that content, while preserving all five required fields.
