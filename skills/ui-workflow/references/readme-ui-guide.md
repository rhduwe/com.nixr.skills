# Main README UI Guide

Use this when creating or updating the main `README.md` UI section. Keep the decisions in `## UI`, or its existing equivalent, and link detailed artifacts from there. This is a field guide, not a separate project guide to generate. Include relevant fields, mark unresolved decisions explicitly, and omit inapplicable fields. Examples are illustrative, not project defaults.

Describe the current UI specification: approved decisions, active proposals,
current artifacts, implementation status, open issues, and validation limits.
Remove superseded alternatives, rejected drafts, and accounts of how the design
changed. Keep acceptance dates, evidence, and preserved revision references only
where they identify the current design or implementation; the UI section is not
a revision history.

## Update the guide in order

1. Locate the existing UI section in the main README; create `## UI` only if no equivalent exists.
2. Read its accepted decisions and update only the requested scope. Establish purpose, users, and platforms before recording a chosen style. Preserve shared/family/item scope and existing acceptance evidence.
3. Add or revise the applicable fields below as decisions are made and artifacts become available. Distinguish Penpot project identity from file identity; do not guess missing links, IDs, values, or defaults.
4. After design review, record the exact accepted design revision and evidence. Record a separately preserved `.penpot` export when available, including its location and date; disclose any limits on version preservation. After implementation review, separately record implementation acceptance and validation results or agreed deferrals.
5. Check that links, inheritance, item mappings, and affected shared consumers agree with the current artifacts and implementation. Keep unresolved or blocked work visible rather than marking it complete. Replace outdated descriptions with the current specification instead of appending change or rejection notes.

For example, adding a runtime settings panel should extend its family's template/item links and status record. It should not create a second UI guide or overwrite an accepted tools/editor density rule.

## Purpose and scope

- Purpose, intended users, and principal tasks.
- Target platforms/devices, separately scoped for runtime and tools/editor UI.
- Whether XR is required and where; scoped constraints and accessibility needs.
- Project-wide supported input methods and their scope; family/item exceptions. Record interaction mappings and switching/loss behavior where relevant. Do not imply every input method works everywhere.

## Penpot locations

| Artifact | What to record |
| --- | --- |
| Penpot team/project | Team and project URLs/IDs where available; explicitly unresolved otherwise |
| Penpot files | Each file URL and file ID, with its role and family scope |
| Shared library | File/page/component/variant links and publishing status where relevant |
| Templates | Exact page/board/component links, names, supported properties and states |
| Item designs | Exact boards, prototype entry points, and related family/template |
| Accepted revision | Version or separately preserved export/snapshot, board references, acceptance date and evidence |

Keep the team, project, file, page, and board identities distinct. Record actual URLs and IDs exposed by the authorized Penpot workspace; leave missing identities unresolved. A mutable board URL alone does not preserve an accepted revision.

## Shared design language

- Palette and semantic color roles, values, contrast/background assumptions, themes, and documented exceptions.
- Typography: families, weights, role/size scale, line height, fallback, and localization needs.
- Spacing and sizing scales, units, responsive/density rules, component and target dimensions, and layout constraints.
- Iconography: source/library, style, sizes, stroke/fill, and labeling rules.
- Motion and depth: feedback, transitions, durations/easing, reduced-motion behavior, and depth/elevation rules where applicable.
- Reusable elements/templates: roles, properties, variants, states, important transitions, and recovery behavior; links to the accepted Penpot definitions.
- XR-specific decisions where applicable: physical dimensions, viewing distance, anchoring, scale mapping from Penpot, reposition/dismiss behavior, and comfort assumptions. Distinguish chosen values, authoritative constraints, and measured device results. Do not derive physical size from pixels alone.

## UI families

Families are this project's related expressions of one design language. Choose only the families the project needs. Keep tools/editor UI distinct from runtime UI; allow runtime subfamilies such as diegetic UI rather than forcing mutually exclusive categories. Do not create a family for every screen.

| Family / parent | Purpose, users and scope | Inherits | Explicit overrides | Penpot library/templates | Implementation locations |
| --- | --- | --- | --- | --- | --- |
| Project-chosen family name and optional parent | Surfaces, platforms, contexts and inputs covered | Shared defaults and parent family | Only the style, layout, behavior or technology differences that apply | Exact links | Actual framework and paths |

Resolve settings as shared defaults -> family override -> item override, including any documented parent-family inheritance. Record the scope and reason of each override; absence of an override inherits a known default, never an invented one.

For example, Runtime and Tools/editor might share typography roles while Tools/editor overrides density. A Diegetic subfamily of Runtime might override world anchoring. These are possible project decisions, not a required taxonomy or assumed values.

Keep three concepts distinct:

- Family: an expression scoped by audience, surface, or use case in this project.
- Theme: an appearance, for example an accepted light/dark appearance where used.
- Component variant: a combination of component properties such as size/state.

Penpot design tokens express reusable values and aliases. Token sets group values and have precedence; token themes select active sets. Component variants represent property combinations. Record the actual mapping and active sets without equating a family to a token theme or variant.

## Implementation and verification

- Actual UI technologies/frameworks by scope, component/template locations, and relevant project skills/conventions.
- Item-to-family/template mapping, implementation paths, and significant local overrides. For shared changes, inventory affected designs and implementations.
- Acceptance criteria and validation approach: visual fidelity, flows/states, accessibility, input methods, supported platforms/devices, and relevant module checks. Record evidence and distinguish specified, demonstrated in Penpot, and verified on device.

| Item / revision | Design status and acceptance evidence | Implementation status and acceptance evidence | Validation scope, results and evidence | Open decisions / agreed deferrals |
| --- | --- | --- | --- | --- |
| Named item and exact revision | Proposed, in review, or explicitly accepted; date and artifact links | Not started, in progress, in review, or explicitly accepted; date and build/capture links | What was checked, where, result, and limitations | Scope, reason, user agreement and follow-up |

Record any permitted Penpot-step bypass with its accepted template, single existing instance, and established property change, confirming no template, shared style, layout, or behavior change. Implementation verification and acceptance remain required.

For example, a label-property change in one accepted button instance can have a documented Penpot bypass while its implementation is still in review. A unique status display with a dedicated Penpot board must instead have that board updated and accepted. Neither record may substitute design acceptance for implementation acceptance.


## Penpot reference links

Use current Penpot documentation for [design tokens, aliases, sets, and themes](https://help.penpot.app/user-guide/design-systems/design-tokens/),
[component variants](https://help.penpot.app/user-guide/design-systems/variants/),
[board-based prototypes and flows](https://help.penpot.app/user-guide/prototyping-testing/prototyping/),
and the [`.penpot` export format](https://help.penpot.app/user-guide/export-import/penpot-file-format/).
Documentation describes product features; verify that the connected version and
current integration expose the operations needed for this task.
