---
name: unity-code
description: Use when writing or modifying any C# code in this project — new scripts, refactors, or reviewing existing code. Establishes code location (Assets/Code/ with Runtime/Editor module folders and per-module asmdefs), namespace conventions, architecture rules (MonoBehaviour vs plain C#, single responsibility, input → logic → presentation splitting, data vs state, size/complexity limits), and Unity code style rules (UniTask over coroutines, no Linq, no FindObjectOfType, explicit dependencies, no per-frame allocations). Triggers on "C#", "script", "class", "code", "MonoBehaviour", "asmdef", "namespace", "code style", "refactor".
---

# Unity Code Conventions

## Code location
- Production and Editor-tool C# code lives under `Assets/Code/`. All project-authored tests, test-only helpers and test assemblies live under `Assets/Tests/`; this is the explicit exception to the code-root rule. Never place authored scripts in `Art`, `Data`, `Plugins`, `Settings`, or the `Assets` root.
- Runtime code goes in `Assets/Code/Runtime/<Module>/`.
- Editor tools go in `Assets/Code/Editor/<Module>/`; Editor-only tests go in `Assets/Tests/EditMode/<Module>/`. No editor-only logic in runtime code — do not use `#if UNITY_EDITOR` in `Assets/Code/Runtime`.
- One folder per module. A module is a cohesive feature domain (e.g. `Player`, `Inventory`, `Combat`). Shared utilities and types used by multiple modules go in a `Common` module.
- Temporary one-off helper scripts go in `Assets/Temp/` and are deleted immediately after use.

## Test code
- Use Unity Test Framework and NUnit test assemblies. All authored tests and test-only helpers/asmdefs belong under
  `Assets/Tests/EditMode/<Module>/` or
  `Assets/Tests/PlayMode/<Module>/`, separate from Editor utilities.
- New tests use namespaces `MoG.Tests.EditMode.<Module>` and
  `MoG.Tests.PlayMode.<Module>`, with module-specific test asmdefs named
  `<Module>.EditMode.Tests.asmdef` and `<Module>.PlayMode.Tests.asmdef`.
- Mark assemblies as Test Assemblies. EditMode assemblies target Editor;
  PlayMode assemblies must compile without UnityEditor or Editor-only helpers.
  Reference the tested runtime assemblies and the required test runner/NUnit
  dependencies explicitly. Production assemblies must not reference tests;
  ordinary player builds must exclude test assemblies.
- Pure rules, persistence fault tests, authoring validation and asset audits
  normally use EditMode. Scene/component lifecycle, spawning, input callbacks
  and runtime bindings need PlayMode integration tests. Use UnityTest when a
  test needs frames or Unity lifecycle execution, not for ordinary assertions.
- Existing suites and their shared helper have been relocated to
  `Assets/Tests/EditMode/`. Their `MoG.Editor.*` namespaces, assembly names and
  asmdef filenames remain unchanged to preserve filters and references; the
  conventions above apply to new suites. Some existing EditMode lifecycle
  cases enter/exit Play Mode and still require Editor-only helpers.
- Run focused automated tests for changed modules and affected dependents.
  Use MPPM by default for routine multiplayer runtime iteration: two players,
  or three for late join, ownership transfer and owner loss. Use the production
  service path for service behavior. Verify distinct UGS accounts per run and
  distinct personal fixture IDs; namespace PlayerPrefs per player or explicitly
  isolate storage. MPPM players share persistentDataPath and a preference store.
- MPPM live-service evidence establishes only the exercised Editor/service
  scenario. Standalone and physical Quest verification remain integration and
  affected-platform gates, including eight-player capacity plus a rejected
  ninth, production timing/network profiles, Meta identity, input/audio,
  suspension and performance. Follow the
  [MPPM run recipe](../../tasks/MultiplayerIntegration.md#mppm-default-iteration).

## UI framework
- All authored runtime UI uses UI Toolkit with Unity App UI. Use App UI controls in UXML and USS for styling; do not add Canvas, UGUI or TMP presentation or compatibility paths.
- Reuse the shared world-space PanelSettings and existing explicit UIDocument bindings. Preserve authored world dimensions and handle visual-tree rebuilds when binding text, buttons or feedback.
- XRI requires the rig's EventSystem/XRUIInputModule infrastructure and transitive UGUI dependencies alongside XRUIToolkitManager. These are input/package dependencies, not permission to add legacy rendering.
- Follow `UIValidation/README.md` for regression checks. Run the retained legacy-UI asset audit when adding or changing UI prefabs/scenes.

## Compilation
- Unity does not perform compilation while in play mode

## Namespaces
- Root namespace matches the project name
- Namespaces mirror the folder path under `Assets/Code/`:
  - `Assets/Code/Runtime/<Module>` → `Root.<Module>`
  - `Assets/Code/Editor/<Module>` → `Root.Editor.<Module>`
- Nested subfolders add nested namespace levels.
- Use `using` directives at the top of the file; do not write fully qualified names inline.

## Assembly definitions
- Every module folder has its own asmdef: `<Module>.asmdef` in `Runtime/<Module>`, `<Module>Editor.asmdef` in `Editor/<Module>`. Create the asmdef together with the module's first script.
- Editor asmdefs reference the runtime asmdef(s) of the module(s) they support.
- Declare explicit asmdef references for every dependency, including third-party code (`UniTask`, `DOTween`) and Unity packages. Do not rely on implicit includes. For Unity packages, reference the package's asmdef **name** (e.g. `Unity.InputSystem`), not the package id (e.g. `com.unity.inputsystem`).
- Never create circular dependencies between modules. If two modules need each other, extract the shared part into a lower-level module.

## Architecture rules

Applies to MonoBehaviours and plain classes alike.

### Single responsibility
- One class = one reason to change. If you cannot state its purpose in one sentence, split it.
- Split by *domain* (per entity/feature) first, then by *operation* (e.g. `Inventory`, `InventoryRules`, `InventorySaver`), then by *layer*.

### MonoBehaviour vs plain C# (engine-facing test)
- A MonoBehaviour owns only engine-facing concerns: `[SerializeField]` references, lifecycle callbacks (`Awake`, `Start`, `Update`, `OnEnable`, `OnTrigger*`), scene wiring, and presentation.
- **Test:** if a method never touches a `UnityEngine` type and never waits for a frame, it does not belong in a MonoBehaviour — move it to a plain C# class.
- Rules with memory (costs, cooldowns, unlock conditions, save snapshots, AI decisions) live in plain C# so they are testable without a scene.
- Plain C# classes take dependencies via constructor or property; MonoBehaviours expose wiring via `[SerializeField]`.

### Splitting an object across components
- When an entity has distinct input, logic, and presentation concerns, split it across up to three MonoBehaviours in a **one-way flow: input → logic → presentation**.
- Naming convention for an entity `Table`:
  - `Table` — main class: owns the logic, the data, and the events.
  - `TablePresenter` — presentation (animation, VFX, audio, UI) driven by the main class's state and events.
  - `TableController` — input (if any): reads input and emits commands to the main class.
- Keep related functionality together; do not split so finely that components call each other for every action.
- 3–7 components per entity is typical; more than 10 is a smell that the design is not cohesive.

### Relationships
- Prefer composition over inheritance. Compose small, focused classes rather than building deep class hierarchies.

### Data vs state
- Keep **authored/definition data** separate from **live runtime state**.
- Definition data (item definitions, tuning values, archetypes, level metadata) is static, authored data. Store it in whatever format fits — a ScriptableObject, JSON, a plain file, etc.
- Museum progression definitions (rooms, exhibits, items, games, requirements and catalog history) use versioned JSON files. Follow [the JSON authoring guide](../../docs/progression-json.md); keep stable IDs in Unity scene/prefab binding components, with no custom ScriptableObject definition hosts. This museum-specific rule does not change Unity/package settings assets or the format choice for unrelated systems.
- Live state (owned items, current health, active cooldowns) lives in runtime objects and is never baked into definition data.

### Size & complexity (guidelines to strive for, not hard limits)
- Aim to stay under the *strive for* column. At the *warning* threshold, stop and consider refactoring using the split rules above.

| Metric | Strive for | Warning |
|--------|-----------|---------|
| Lines per file | 300 | 200 |
| Lines per method | 60 | 40 |
| Methods per class | 15 | — |
| Fields per class | 15 | — |
| Parameters per method | 5 | — |
| Nesting depth | 4 | — |
| Cyclomatic complexity | 12 | — |

## Code style rules
- This is code for Unity
- For private variables use names starting with underscore
- Expose variables for Unity editor using [SerializeField] attribute in one line
- Don't use single line if statement
- If the code has to be asyncronous, use async execution based on UniTask library instead of Coroutines
- Public properties should be capitalized 
- For serializable files that don't inherit from MonoBehaviour don't use SerializeField. Fields can be public. If this class will be deserialized from json, add JsonProperty attribute. 
- Don't use Linq
- Don't use RequireComponent attribute
- If the class needs events, use EventHandler and specialized EventArgs classes
- Use explicit types instead of var
- Class members order (from top to bottom): events, public fields, non-public fields with SerializeField attribute, public properties, private and protected properties, private fields.
- Don't make null checks for fields exposed in inspector
- When subscribing to C# events, name the handler methods like this: EventDispatchingClass_EventName
- Only add comments to non-trivial parts of code and methods
- Don't use GetComponent and similar methods. If there is a requirement for a dependency, use a field with [SerializeField]. TryGetComponent and similar are acceptable when we are dealing with objects not available at initialization.
- Use new() and simplified object initialization syntax 
- Don't add any comments, except for technically-complex code and non-obvious behavior
- One public class per file; the file name must match the public class name
- No per-frame allocations: in `Update`, `FixedUpdate`, and other per-frame paths do not create new objects, concatenate strings, or allocate via closures
- Don't use `FindObjectOfType`, `FindObjectsByType`, or `GameObject.Find`. All dependencies are explicit references (`[SerializeField]` fields or constructor/property injection)
- Prefer configurable variables exposed in the editor (`[SerializeField]`) over hard-coded values
