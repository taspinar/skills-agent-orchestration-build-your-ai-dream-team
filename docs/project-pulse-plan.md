# Project Pulse implementation plan

## Goal

Build Mona's Project Pulse as a lightweight, polished static dashboard for contributors. The first view should make it easy to understand which projects are active, who owns them, their current status, recent activity, and priority or risk level. The interface should use readable spacing, visible project cards, clear status badges, accessible structure, and responsive behavior.

## Agent responsibilities

### Planner

- Research the repository, brief, existing conventions, and launch requirements.
- Define the implementation phases, file ownership, dependencies, parallel work decisions, edge cases, and validation criteria.
- Surface uncertainty or open questions before implementation begins.

### Orchestrator

- Convert this plan into ordered execution phases.
- Assign explicit file scopes so agents do not overwrite one another's work.
- Coordinate Designer and Coder work, run independent tasks in parallel where safe, and sequence dependent work.
- Review the integrated dashboard and confirm that the required files and launch configuration work together.

### Designer

- Own the visual and interaction design for the dashboard within the assigned files.
- Define the information hierarchy for the page header, project summary content, project cards, status badges, owner/activity details, and priority treatment.
- Ensure accessible color contrast, semantic structure guidance, readable typography, keyboard-friendly interactions, responsive layout, and clear visual affordances.
- Implement or specify polished styling in `app/styles.css`, including the `.dashboard` and `.project-card` hooks, rounded corners, shadows, status styles, and mobile behavior.

### Coder

- Implement the static dashboard in the assigned files while following the Designer's direction and this plan.
- Build `app/index.html` with semantic, accessible markup and a clear Project Pulse first view.
- Create `app/project-data.json` with a top-level `projects` array. Each project must include `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Wire the page to the data without introducing unnecessary dependencies; if the page uses client-side loading, handle loading and data errors explicitly.
- Create `.vscode/launch.json` as strict JSON with no comments. Configure the `Run Project Pulse Dashboard` launch to serve from `${workspaceFolder}/app` and open `index.html`, not a directory listing.
- Validate the implementation before handing it back to the Orchestrator.

## File assignments

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder, using Designer's structure guidance | Create the dashboard document, semantic content structure, project card markup, status and priority presentation, and references to the stylesheet and project data. |
| `app/styles.css` | Designer | Create the polished responsive visual system, including `.dashboard`, `.project-card`, badges, spacing, contrast, focus states, and responsive layout. Coder should only adjust it when required to wire or correct the assigned UI. |
| `app/project-data.json` | Coder, with content requirements from Planner and Designer | Provide representative contributor-friendly project records under a top-level `projects` array with all required fields and realistic status, activity, and priority values. |
| `.vscode/launch.json` | Coder | Add the deterministic `Run Project Pulse Dashboard` launch configuration, using `${workspaceFolder}/app` as the working directory or server root as appropriate and opening `index.html`. |
| `docs/project-pulse-plan.md` | Planner | Preserve the implementation plan and its coordination criteria; do not modify it during feature implementation unless the Orchestrator explicitly approves a plan update. |

## Implementation phases and dependencies

1. **Confirm requirements and ownership — Planner and Orchestrator**
   - Confirm the brief, required fields, visual hooks, launch behavior, and validation gates.
   - Dependency: none.

2. **Define dashboard structure and visual direction — Designer**
   - Establish the information hierarchy, responsive layout, accessibility decisions, card and badge treatment, and CSS hooks.
   - Assignments: `app/styles.css` and structural guidance for `app/index.html`.
   - Dependency: Phase 1.

3. **Create the project data model — Coder**
   - Create representative project records with the required schema and values that support the dashboard's status, activity, ownership, and priority views.
   - Assignment: `app/project-data.json`.
   - Dependencies: Phase 1; the data field names must align with the HTML rendering approach.

4. **Implement the dashboard page — Coder**
   - Build the semantic page and project card presentation, connect the data, and apply the Designer's layout and accessibility direction.
   - Assignment: `app/index.html`; coordinate with `app/styles.css` and `app/project-data.json`.
   - Dependencies: Phase 2 for design direction and Phase 3 for the data schema.

5. **Configure the runnable experience — Coder**
   - Create the launch configuration so the app is served from `app/` and opens `index.html`.
   - Assignment: `.vscode/launch.json`.
   - Dependency: Phase 4, so the launch target and working directory can be checked against the final app structure.

6. **Integrate and validate — Orchestrator with Designer and Coder**
   - Review all assigned files together, resolve integration issues, and confirm the dashboard meets the brief.
   - Dependencies: Phases 2 through 5.

## Parallel work decisions

- Phase 2 (Designer visual direction) and Phase 3 (Coder project data) may run in parallel after Phase 1 because they own different files and have no direct file overlap.
- The Designer may refine `app/styles.css` in parallel with the Coder creating `app/project-data.json`, but the Orchestrator must keep each agent's file scope explicit.
- `app/index.html` must wait for the data schema and the Designer's structural direction; otherwise markup and field names may diverge.
- `.vscode/launch.json` can be drafted in parallel with late UI polish, but final launch validation must run after `app/index.html` exists and its location is stable.
- Integration and validation must be sequential after all implementation outputs are available. The Orchestrator should not hand off a partially integrated dashboard.

## Validation expectations

- Confirm all required files exist: `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.
- Parse `app/project-data.json` and verify it has a top-level `projects` array; verify every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `app/index.html` references the stylesheet and data, contains the Project Pulse dashboard view, and includes visible project-card markup and status/priority information.
- Confirm `app/styles.css` contains `.dashboard` and `.project-card`, with readable spacing, responsive behavior, rounded corners, shadows, contrast, and focus-visible treatment.
- Parse `.vscode/launch.json` as strict JSON and verify it contains the exact launch name `Run Project Pulse Dashboard`, serves from the `app` directory, and opens `index.html`.
- Run the existing repository validation where applicable and inspect the dashboard in a browser or preview so the launch opens the UI rather than a server directory listing.
- Check responsive behavior at narrow and wide viewport sizes, keyboard focus visibility, semantic headings/labels, and that missing or malformed data does not produce a silent success-shaped failure.

## Risks and edge cases

- Keep the JSON schema and HTML field names synchronized; a renamed field can leave cards partially empty.
- Avoid relying on fragile external services or build steps for this small static app.
- Ensure status and priority are not conveyed by color alone.
- Prevent long owner, activity, or project names from breaking the card layout.
- If data loading fails, present an explicit user-facing error state rather than an empty dashboard that appears valid.
- Ensure the launch configuration's server root and URL target are both aligned with `app/index.html`.

## Handoff criteria

The Orchestrator can hand off the dashboard when the four assigned files are present, the data and launch configuration pass structural validation, the UI visibly presents the required Project Pulse information, and the Designer and Coder have confirmed that their work integrates cleanly.
