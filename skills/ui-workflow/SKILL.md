---
name: ui-workflow
description: Use only when designing, creating, or changing visual UI elements for runtime or tools/editor interfaces. Coordinate visual styles, Penpot design, implementation, and separate design and implemented-UI acceptance. Excludes behavior-only changes that preserve the visual design.
---

# UI Workflow

Coordinate a project's visual design language, Penpot artifacts, and implementation of visual UI elements. Select specialist skills and implementation technology from the actual project; no engine, framework, platform, or family taxonomy is prescribed here.

## Design task boundaries and commits

A single design task includes the initial proposal, all review iterations,
user feedback, corrections and final acceptance. A new message, agent turn
or review iteration does not start a new design task.

Do not create commits while that design task remains in progress.
This restriction includes specifications, README changes, previews and
other supporting files. Keep permitted repository edits uncommitted.
Delegated steps and agent handoffs do not create commit boundaries within the task.

Complete the task only after explicit user acceptance, proposal cleanup,
accepted artifact preservation and documentation checks. Then commit the
completed design task as one logical change.

Do not treat specification acceptance, style acceptance or approval of one
correction as acceptance of the complete screen design.

## When to use

- Establishing a project's visual UI foundation, style, or UI families.
- Designing or creating visual UI elements, from one element to a screen or visual flow, for runtime or tools/editor use.
- Changing existing visual UI elements, including their appearance, content, layout, or visual state design.
- Changing the visual design of shared styles, components, or templates and updating affected uses.
- Producing a visual UI design-only deliverable through explicit acceptance.

Apply this workflow only when the requested work designs, creates, or changes visual elements. Behavior-only work is outside its scope when it preserves existing visual elements, appearance, layout, and visual state design. Examples include event wiring, completion logic, state restoration, feedback timing, and audio triggers, including when they trigger an already-designed animation. Such work alone must not invoke Penpot, create design boards, or require this skill's design or implemented-UI acceptance gates.

An actual visual design task still includes documenting and verifying the behavior, states, and transitions needed to explain that design. These supporting details do not independently make a behavior-only task a visual design task. The workflows and gates below apply within this visual scope. Examples below illustrate decisions and routing; they do not establish project defaults.

## Start every visual UI task

1. Read the main `README.md` section `## UI`, or its existing equivalent, if present. Before creating or editing `README.md`, establish an explicit shared understanding with the user of the project goals, intended users, principal tasks, UI scope, and target visual style. Reuse prior explicit agreement; sourced constraints, observed implementation, or agent assumptions alone do not establish this agreement. If the README is missing, leave it absent until this prerequisite is met. Then keep the current UI specification in that canonical section and link detailed artifacts from it; do not create a duplicate section or a separate default UI guide. Record current approved decisions, active proposals, artifacts, open issues, and validation limits; remove superseded alternatives, rejected drafts, and change chronology. Retain acceptance evidence and preserved revision references for the current design and implementation. Use [the README UI guide](references/readme-ui-guide.md) when establishing or extending its fields. This prerequisite does not replace the later Penpot design acceptance or separate implemented-UI acceptance.
2. Identify the requested scope: foundation, new item, existing item, or shared change; determine whether the deliverable is design-only or includes implementation. Read accepted Penpot artifacts and existing implementation as needed. Reuse settled answers and authorizations; clarify only unresolved choices that affect the task.
3. Classify the UI family and resolve shared defaults -> family overrides -> item overrides, including documented parent-family inheritance. Keep each decision's scope explicit. Never invent a missing default or promote a local choice to project policy.
4. Discover and read applicable specialist skills/plugins. Use the available Penpot integration and confirm the required reading, editing, prototyping, inspection, and sharing capabilities. Follow the Penpot access guidance below. Read access does not establish write access. Report exact capability gaps, continue independent specification work, and identify blocked deliverables. Do not silently install plugins, invent completed edits, or substitute an unrequested artifact for Penpot.
5. Follow the relevant ordered workflow below. If the task is design-only, use the design-only stopping workflow instead of continuing into implementation.

**UI families** are project-chosen related expressions of a shared design language, not a universal standard. Keep tools/editor families distinct from runtime families where applicable. Diegetic UI can be a runtime subfamily; these labels need not be mutually exclusive. Record parent relationships and scoped differences. Themes describe appearances; component variants describe properties such as size or state. Penpot token sets group reusable values, while token themes activate combinations of sets. Neither has an automatic one-to-one mapping to a UI family.

For example, a project might choose Runtime, Tools/editor, and a Diegetic subfamily of Runtime. They could share semantic colors while Tools/editor overrides density and Diegetic overrides its presentation in the world. Record only choices actually made for that project.

## Penpot access and artifacts

Use the authorized Penpot team/project/file and read the current design before
editing. If Penpot MCP is available, read `high_level_overview` before design
work and request the relevant types through `penpot_api_info` before using
`execute_code`. The editor file must be connected through the Penpot MCP Plugin;
an installed server alone does not connect an account or file. Follow the
returned API documentation rather than assuming another design tool's methods.
Browser editing or the documented Penpot Plugin API can support the same
workflow when available; verify their actual capabilities and access first.

Use boards for layouts and prototype screens, flex/grid layouts when appropriate,
local or connected library components and variants for reusable elements, and
design tokens with aliases, sets, and themes for shared values. Verify bindings
and component propagation after edits; literal styling is not a token binding.
Inspect actual output through `export_shape` when available and walk the
prototype separately. An exported image proves only the rendered appearance.
Preserve accepted revisions through available history or a separately saved
`.penpot` export, recording the exact board IDs, revision, and acceptance evidence.
A locked board or mutable link alone does not preserve a revision. State
preservation limits accurately; do not claim an immutable cloud version from an
editable export or snapshot. Exported tokens do not establish engine integration.

### Download native Penpot files only after design acceptance

Do not request a native export, download a `.penpot` archive or commit one
before the user explicitly accepts the current design.

This restriction applies to temporary directories, staging files, proposal
archives, backups and canonical exports. Structural validation and
preservation are not exceptions.

Before acceptance, inspect the design through Penpot, prototype links and
rendered image previews. Existing accepted archives may be read.

After acceptance, complete proposal cleanup and follow
[the native download procedure](references/penpot-download.md).
Check the exported archive before replacing the canonical file.
Record the accepted boards, acceptance evidence and export identity.

### Reusable Penpot foundation

- Keep tokens, styles, components, and prototype boards in one foundation file
  unless the project needs separate files or a published library.
- Build reusable components with named properties and necessary state variants;
  compose panels from linked components.
- Reuse linked instances. Define permitted content, variant, and layout
  overrides; change shared styling at its definition and avoid detaching.

When required by the task or supported by actual component examples, express
shared -> family -> item rules explicitly, demonstrate states and prototype
flows, and inspect affected instances, wrapping, states, and flows after shared
changes. Do not assume automatic inheritance or build speculative family
examples and flows solely to complete a checklist.

### Token definition, binding and verification

- Define tokens for shared visual decisions, including typography, colors,
  spacing, sizes, radii and strokes where reuse makes sense. Use primitive
  values and semantic roles. Keep one-off values literal instead of creating
  tokens for every property. Keep proposed values distinct from accepted decisions.
- Record the token source, active sets and their order. The last active set
  wins when names overlap. Use themes for meaningful combinations of sets.
  Do not require a theme for every UI family.
- Use composite typography tokens for shared text roles in token-based
  designs. Define family, size, weight and line height explicitly or through
  references. Typography tokens and typography styles replace each other on
  a text layer. Do not maintain competing definitions on the same layer.
- Bind tokens to actual shared component properties. Check affected instances
  after component propagation and structural edits. Inspect property-to-token
  bindings and resolved values. Matching literals or token counts do not
  demonstrate correct use. Check semantic roles, such as entered text versus
  placeholders. Tokens do not replace component structure or layout behavior.
- Establish compatible properties before applying tokens. Gap tokens require
  flex or grid layout. Stroke-width tokens require an existing stroke.
  Check the installed version's supported token types and application methods.
- When establishing or migrating tokens, change and restore representative
  tokens to check propagation. Inspect main components, instances and rendered
  screens for unintended wrapping, alignment or appearance changes. Inspect
  existing bindings before repeating API application. Check the installed
  integration's behavior instead of assuming repeated application is harmless.
- Document where tokens live, the active configuration and intentional local
  overrides. For shared-library work, check installed-version support and edit
  tokens in their source file. Penpot currently permits one token source per
  file: local tokens or one connected library. Exported tokens alone do not
  establish implementation integration.

Use the current [token manual](https://help.penpot.app/user-guide/design-systems/design-tokens/)
and [typography guidance](https://penpot.app/blog/tutorial-creating-and-applying-typography-design-tokens/)
for token definitions and application rules. For automation, check the
[binding API](https://doc.plugins.penpot.app/interfaces/ShapeBase#tokens).
For multi-file designs, check the
[shared-library guidance](https://help.penpot.app/user-guide/design-systems/libraries/#use-tokens-from-a-shared-library).

### Board scope and reuse

- Justify separate boards. Create a board when it demonstrates a distinct
  layout, available action or important transition.
- Keep content examples compact. Use component examples for empty, populated,
  focused and maximum-length cases when the surrounding screen stays the same.
- Reuse prototype states. Do not duplicate screens solely to support different
  incoming paths or return destinations. Use supported overlays or navigation
  where appropriate.

## Inspect existing projects without prescribing their future

Treat the current implementation and repository documentation as evidence of
what exists, not automatic rules or constraints for the desired design.
Before carrying existing choices into the design, present a concise inventory
of relevant appearance, behavior, platforms, inputs, frameworks, and layout
choices. Ask the user which parts to retain, change, or leave open. Distinguish
observed implementation, documented requirements, and choices explicitly
confirmed for this design task. Reuse those confirmations without asking again.
Do not let existing fonts, colors, components, or technology silently narrow
style alternatives or become project policy. Explain genuine feasibility
constraints with evidence rather than inferring them from current usage.
Continue work independent of unresolved choices while awaiting the answer.

For example, finding purple panels, serif headings, and controller input does
not require the new design to preserve them. Present those findings and ask
which belong in the intended design before treating them as requirements.

## Choose specialist workflows

1. For spatial 2D XR/MR panels, including occasional depth controls supporting panel interaction, use `xr-panel-design` when available. For a Meta target, use `hz-immersive-designer` when appropriate and follow applicable platform verification skills. Do not transfer Meta assumptions to other platforms.
2. For non-XR interfaces, use suitable available Penpot design skills/plugins. Route general 3D interaction outside panels to appropriate design skills; `xr-panel-design` excludes general 3D object design and engine implementation.
3. For implementation, follow the project's actual UI technology and code/asset conventions through applicable skills. In a Unity project, for example, `unity:ui` routes to the existing framework: `unity:ui-uitk` for UI Toolkit, `unity:ui-ugui` for uGUI, or `unity:ui-imgui` for existing legacy IMGUI. Do not choose Unity for an unrelated project.

For example, route an XR runtime settings panel to `xr-panel-design` for Penpot work, and a desktop editor inspector to non-XR Penpot tooling. Implement the inspector with the framework already used by its project.

## Proposals and acceptance

### Browser links for review

Include a clickable HTTP(S) link in every style, design and implementation review request, including each revision.
The link must open the current Penpot board, prototype, running UI or review captures in the user's default browser.
Images and file links do not replace it.

Check the destination and state any access requirements.
If the link is unavailable, report the blocker, next action and owner.

### Complete an accepted design

After explicit acceptance of the complete design:

1. Record the acceptance and identify the accepted boards.
2. Keep accepted elements on the page appropriate to their purpose. Multiple pages are allowed.
   Move an element if it belongs on another page. Rename a proposal page if it becomes a permanent design page.
3. Remove rejected alternatives, proposal-only annotations, temporary review controls and previews.
   Remove pages that no longer serve a purpose. Preserve accepted screens, states and useful prototype interactions.
4. Check that cleanup preserves the accepted appearance and behavior.
5. Download and check the final native archive.
6. Update the canonical archive and README UI record.
7. Check the complete repository diff and commit the finished design task as one logical change.

Acceptance cleanup removes temporary proposal material.
It does not require consolidating accepted designs onto one page.

If cleanup requires a substantive design change, return to user review.
Do not download the revised archive or commit until the user accepts the revised design.

## Workflow: Establish the UI foundation

1. Establish the project's purpose, users, principal tasks, and target platforms before presenting style alternatives. Identify runtime and tools/editor needs, whether and where XR is required, supported input methods and their scope, accessibility needs, and relevant constraints. Apply the existing-project inspection rule above; reuse choices explicitly confirmed for the desired design and resolve only missing choices needed for it.
2. Present meaningful visual style alternatives suited to that purpose, using representative examples in Penpot and concrete tradeoffs. Apply user feedback and repeat until the user explicitly accepts the style.
3. Build the shared elements and reusable templates in Penpot with required properties, variants, states, and important flows. Define scoped family differences rather than silently diverging from the shared language. Run the Penpot design review loop below until these designs are explicitly accepted.
4. Record the accepted design language, inheritance, input/platform scope, Penpot project/file identities, library/template locations, and design status in the main README UI section. Follow the reference field guide; keep implementation and validation status separate.
5. If implementation is in scope, run the implementation and acceptance loop below for the agreed shared implementation. Otherwise use the design-only stopping workflow.

For example, after establishing that a product serves headset users and desktop content authors, compare visual directions using a runtime panel and an authoring form. Once a style is accepted, build its button, field, and panel templates with their required states. A style chosen for this example is not a default for another project.

## Workflow: Create a new UI item

1. Read the applicable family rules and accepted templates. Establish the item's purpose, users, entry/exit points, content, supported inputs, flows, required states, recovery paths, and acceptance criteria.
2. If the necessary foundation is missing, establish the relevant foundation first. Reuse accepted templates where suitable; create the item's Penpot design even when it will be a unique element.
3. Run the Penpot design review loop until the user explicitly accepts the item design. Record its family/template mapping and accepted revision in the README UI section.
4. If implementation is in scope, run the implementation and acceptance loop. Otherwise use the design-only stopping workflow.

For example, design an XR runtime settings panel with its applicable focus, pressed, disabled, saving, and error states. Demonstrate the relevant transitions in Penpot, obtain design acceptance, then implement with the project's chosen framework and obtain separate acceptance of the running UI.

## Workflow: Modify an existing UI item

1. Inspect its current implementation, family rules, accepted template or dedicated Penpot counterpart, and the requested change. Establish which properties, states, flows, and dependencies are affected.
2. If the change alters a shared style or template, use the shared-change workflow below. Otherwise evaluate the narrow Penpot exception below; do not infer eligibility from the change being small.
3. When the exception does not apply, update the dedicated Penpot counterpart or create a design if none exists, and run the Penpot design review loop. When it does apply, record the accepted template, single instance, and permitted property change that justify bypassing the new Penpot edit/review.
4. Run the implementation and acceptance loop for implementation work. The Penpot exception never removes verification or explicit implemented-UI review. For a design-only edit, use the design-only stopping workflow.

### Narrow Penpot exception

This exception applies only to visual changes already within this skill's scope. Behavior-only work is excluded by the scope rule above and does not need this exception.

Allow a bypass only for a local change to **one existing element instance** of an accepted Penpot template, entirely within that template's established properties, with no template, shared style, layout, or behavior change. Changes to a template always require Penpot. A unique/singular element with its own dedicated Penpot design always has that counterpart updated and reviewed. Without an accepted template or counterpart, create the Penpot design normally. If classification is ambiguous, do not assume the exception applies.

For example, changing one existing button instance's label from "Start" to "Continue" can qualify if its accepted template already exposes that text property and the change preserves layout and behavior. Increasing the shared button template's padding requires Penpot. Editing a one-off status display with its own Penpot board also requires updating and reviewing that board, even if only one element changes.

## Workflow: Change shared styles or templates

1. Identify the shared definition and inventory affected families, components, screens, Penpot designs, and implementation locations. Resolve inheritance and overrides to distinguish actual consumers from unrelated families.
2. Establish the change's purpose and acceptance criteria. For a change of visual direction, present relevant alternatives and iterate until the style is explicitly accepted; preserve unchanged accepted decisions.
3. Update the shared Penpot definition and affected designs, states, and flows. Run the Penpot design review loop for the changed scope; shared template changes never use the instance exception.
4. Update the README UI section with the revised definitions, scoped overrides, artifact links, and impact inventory.
5. If implementation is in scope, update the affected shared implementation and its consumers, then run the implementation and acceptance loop across that impact scope. Leave unrelated families outside the change unless the user includes them. Otherwise use the design-only stopping workflow.

For example, changing a shared semantic warning color requires checking runtime panels and tools that inherit that role. A diegetic subfamily with an explicit independent override may be unaffected; confirm its mapping before including it. Update affected Penpot designs before implementing the accepted revision.

## Shared loop: Penpot design and user acceptance

1. Create or revise the relevant Penpot elements, layouts, states, and important transitions with applicable specialist skills/plugins. Include supported inputs, feedback, cancellation, and recovery as relevant.
2. Inspect the boards and available prototype paths. Check applicable token bindings, resolved values and affected component instances under the token rules above. Follow the browser-link requirement for every review request. Present exact board/prototype links, the revision, changed behavior, and unresolved choices. Distinguish static/annotated, clickable, or animated artifact behavior from validation status: specified, demonstrated in Penpot, or verified on device.
3. Apply feedback, recheck affected states/flows, and repeat presentation and user review until the user explicitly accepts the current design. Silence or praise alone is not acceptance. Preserve authorization for unchanged, already-approved work.
4. Record the accepted revision, exact boards, acceptance date, and user acceptance. Preserve a version or snapshot when supported; disclose any preservation limitation rather than claiming a mutable board link is immutable.

For example, a clickable saving/error flow can demonstrate navigation and recovery in Penpot. It does not establish that a physical XR press or device input has been verified.

## Shared loop: Implementation, verification, and user acceptance

1. Implement the accepted design using applicable project skills and existing UI technology. If implementation requires a design change, return to the Penpot loop for that change before implementing it.
2. Verify visual fidelity and behavior against the accepted revision, including affected states, transitions, inputs, recovery, and accessibility needs. Run checks for changed modules and affected dependent modules. Broaden testing only for failures or uncertain impact; do not require the full suite for every UI change.
3. Perform applicable target-device validation. Record test scope, evidence, results, and limitations, keeping specification, Penpot demonstration, and device verification distinct. Finish required validation or record a consciously agreed validation deferral with its scope, reason, and user agreement.
4. Present the implemented UI through suitable captures and/or an accessible running build for explicit user review. Include a default-browser review link as required above. Apply feedback, verify affected behavior, and repeat until the user explicitly accepts the implementation. If feedback requires a design change, return to Penpot and then repeat implementation verification and review.
5. Update the README UI section with implementation locations, separate design and implementation acceptance records, and validation evidence or agreed deferrals. Complete the implementation task only after explicit acceptance of the implemented UI; report blocked or unaccepted work accurately.

For example, after implementing an editor form, show the actual form and demonstrate validation errors and recovery. Acceptance of its Penpot boards does not accept the running form; request implementation acceptance after the user can review it.

## Workflow: Finish a design-only task

1. Complete the applicable foundation, new-item, existing-item, or shared-change design steps, including the Penpot design review loop. Do not proceed into implementation merely because a design is accepted.
2. Record the accepted style/design revision, exact artifact links, family/template mapping, and acceptance evidence in the README UI section. Mark implementation and device validation accurately as unperformed, existing, or otherwise evidenced; do not infer completion.
3. Deliver the accepted design and summarize remaining implementation decisions and validation limitations within the requested scope.

For example, a request to design a diegetic runtime panel ends at the explicitly accepted Penpot design and documented spatial/input specifications. It does not authorize building the panel in an engine.
