# Project Pulse Dashboard Implementation Plan

## Goal

Build a lightweight, polished, accessible static **Project Pulse** dashboard. The dashboard must make project names, owners, statuses, recent activity, priorities, and contributor-friendly summaries easy to scan across desktop and narrow viewports.

## Ordered phases

### Phase 1: Planning and design handoff

1. **Orchestrator** asks the Planner to define the implementation approach, acceptance criteria, dependencies, and file ownership.
2. **Designer** defines:
   - Information hierarchy for project identity, ownership, status, recent activity, priority, and summaries.
   - Responsive layout behavior and card composition.
   - Accessibility requirements, including semantic structure, keyboard access, visible focus, contrast, and reduced-motion behavior.
   - Status badges, priority treatment, typography, spacing, card styling, and interaction states.
   - Deterministic CSS hooks, including `.dashboard` and `.project-card`, so implementation and validation can target stable selectors.
3. The Planner records the agreed data shape and implementation boundaries before UI rendering begins.

### Phase 2: Parallel preparation

Designer and Coder preparation may proceed in parallel because their responsibilities do not overlap:

- **Designer** finalizes visual, responsive, and accessibility decisions for the implementation handoff.
- **Coder** prepares the data shape and launch configuration without making assumptions about unfinished visual decisions.
- **Coder** owns `.vscode/launch.json`. It must be strict JSON, use `"cwd": "${workspaceFolder}/app"`, launch `python3 -m http.server 5500`, and open a URL ending in `/index.html`.

### Phase 3: Sequential implementation

After the Designer’s decisions are available, the Coder creates and wires the application:

1. Create the valid project data in `app/project-data.json`.
2. Implement the semantic dashboard shell and rendering in `app/index.html`.
3. Implement the Designer-approved visual system and responsive behavior in `app/styles.css`.
4. Confirm the launch configuration serves the app and opens `index.html` rather than a directory listing.

### Phase 4: Launch and browser validation

1. Start the configured preview server.
2. Confirm `/index.html` returns the dashboard, not a directory listing.
3. Inspect the dashboard at desktop and narrow viewport widths.
4. Exercise keyboard navigation and visible focus states.
5. Confirm every rendered project card exposes status, recent activity, and priority.

### Phase 5: Orchestrator review

The Orchestrator reviews the integrated result against the acceptance criteria, ownership assignments, accessibility decisions, data contract, launch behavior, and validation evidence. Any issue found is routed to the owning role before completion.

## File assignments

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder, guided by Designer | Build the semantic dashboard structure; use the exact title `Project Pulse`; reference the stylesheet and JSON data; load and render data; expose visible `.project-card` elements; render each project’s name, owner, status, recent activity, and priority. |
| `app/styles.css` | Designer specifies; Coder implements | Implement `.dashboard` and `.project-card`, typography, spacing, responsive behavior, status and priority affordances, border radius, box shadow, sufficient contrast, visible focus states, and reduced-motion-friendly behavior. |
| `app/project-data.json` | Coder | Provide valid JSON with a top-level `projects` array. Every project item must contain `name`, `owner`, `status`, `recentActivity`, and `priority`. |
| `.vscode/launch.json` | Coder | Provide strict JSON for `Run Project Pulse Dashboard`; serve from the `app` directory with `python3 -m http.server 5500`, set `cwd` to `${workspaceFolder}/app`, and open `index.html` rather than a directory listing. |

## Designer responsibilities

- Establish a clear scan order: project name and summary first, owner and status next, then recent activity and priority.
- Define a responsive card grid or equivalent layout that remains readable on narrow screens.
- Specify status badges and priority treatments that do not depend on color alone.
- Specify semantic and accessible labeling for status, priority, activity, and project summaries.
- Define type scale, spacing rhythm, card boundaries, border radius, shadows, contrast, and focus treatment.
- Ensure the design remains usable with keyboard navigation, high-contrast conditions, long text, and reduced-motion preferences.
- Provide the deterministic class hooks `.dashboard` and `.project-card` and any additional hooks needed for status or priority variants.

## Coder responsibilities

- Preserve the agreed data contract and keep rendering resilient to empty, malformed, or incomplete records.
- Implement semantic HTML and accessible names, headings, landmarks, and labels.
- Keep the exact document title `Project Pulse`.
- Reference `app/styles.css` and `app/project-data.json` correctly from `app/index.html`.
- Render visible cards with all required project fields and contributor-friendly summaries.
- Implement the Designer’s CSS decisions without changing the agreed information hierarchy.
- Add explicit, understandable handling for unknown status or priority values rather than silently hiding them.
- Keep `.vscode/launch.json` valid JSON and aligned with the required server command, working directory, and URL.

## Dependencies

- The data shape must be agreed before UI rendering is implemented.
- The Designer’s information hierarchy and accessibility decisions must precede final HTML and CSS implementation.
- `app/index.html` depends on both `app/styles.css` and `app/project-data.json`.
- Launch validation depends on all application files being present and correctly referenced.
- Browser validation follows completion of all app files and the launch configuration.

The required sequence is:

**Planner → Designer/Coder handoff → Coder integration → launch/browser validation → Orchestrator review**

## Parallel work decisions

Designer visual/accessibility preparation and Coder data-shape/launch-configuration preparation are safe to perform in parallel because they affect separate concerns and files. Final Coder integration remains sequential after the Designer handoff so that HTML structure and CSS behavior implement one agreed hierarchy and accessibility model. Launch and browser validation remain after integration because they depend on the complete set of app files.

## Edge cases and expected handling

- **Empty data:** Render a clear contributor-friendly empty state without breaking the dashboard shell.
- **Malformed data:** Surface a clear, non-silent error state; do not pretend that invalid data rendered successfully.
- **Missing fields:** Use an explicit unavailable treatment or fallback label while retaining the card and its accessible structure.
- **Long text:** Allow names, owners, activity, and summaries to wrap without clipping or causing horizontal overflow.
- **Unknown status or priority:** Use a neutral, legible fallback treatment that remains understandable without color.
- **Narrow viewports:** Reflow cards and metadata without requiring horizontal scrolling.
- **Keyboard navigation:** Ensure interactive elements, if present, are reachable in a logical order.
- **Visible focus:** Provide a clearly distinguishable focus indicator with adequate contrast.
- **Contrast:** Preserve readable text and meaningful status/priority distinctions in all states.
- **Directory listing:** Configure and verify the preview so the browser opens `/index.html`, not a server directory listing.

## Validation expectations

Run the following targeted checks after implementation:

1. Validate data JSON:
   ```bash
   python3 -m json.tool app/project-data.json
   ```
2. Validate launch JSON:
   ```bash
   python3 -m json.tool .vscode/launch.json
   ```
3. Inspect targeted implementation details:
   - `.dashboard` and `.project-card` selectors exist.
   - The required project fields are present in the data and rendered by the page.
   - `app/index.html` contains the exact `Project Pulse` title.
   - Stylesheet and JSON references resolve to the expected files.
   - Launch configuration contains `Run Project Pulse Dashboard`, `cwd: "${workspaceFolder}/app"`, `python3 -m http.server 5500`, and a URL ending in `/index.html`.
4. Start the preview server and confirm requesting `/index.html` returns the dashboard document rather than a directory listing.
5. Inspect responsive layout at narrow and wide widths, including long text and empty-state behavior.
6. Navigate with the keyboard and confirm focus remains visible and logical.
7. Confirm every rendered `.project-card` exposes status, recent activity, and priority in accessible, readable content.
