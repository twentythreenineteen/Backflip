# FAP engineering and UX

Use with [pixel-safe layout](layout.md); check the actual target firmware's headers, exported SDK, project conventions and app requirements first. Firmware forks and versions may differ.

## Project and compatibility

- Inspect `application.fam`, the app entry point and existing modules before changing structure. Use a unique app ID and the external app type, correct entry point, stack budget and any appropriate version, icon, asset and category fields supported by the target SDK. Keep asset names and generated headers consistent with the project's build.
- Target a known firmware/SDK release or commit. Check that functions and hardware features are public and exported to external apps, not merely present in firmware source; FAP ABI and firmware API compatibility matter. A successful build against one SDK does not establish compatibility with all official releases or forks.
- Use the project's `ufbt`/FBT build and deployment workflow. Where supported, build against the actual firmware SDK and report its version, commands and results. Avoid unnecessarily requesting permissions or accessing a radio/peripheral, storage or network without the app's explicit purpose.

## UI and input

- Prefer built-in UI modules for menus, dialogs, variable lists, text input and text display when they satisfy the screen. Choose a single view for simple apps; use a view dispatcher and scenes where multiple views/state transitions justify them. Model navigation and transitions explicitly, with Back, cancellation and failure routes.
- A custom view should draw from a coherent model. Separate rendering from state changes, avoid blocking operations in draw/input callbacks, and request redraws when model data changes. Long or repeated key events are distinct from short presses; make behavior deliberate and prevent repeated actions where unsafe.
- Keep progress, empty, success and actionable error states readable and escapable. Decide what happens on cancel, device disconnect, missing file or unexpected input; avoid controls hidden underneath overlays. Maintain appropriate contrast and recognizable labels.

## Ownership, concurrency and failures

- Identify each object's owner, callback context lifetime and acquisition/cleanup order. Allocate required views, records, workers and resources; check failures wherever the applicable API can return them. On every exit or partial-initialization failure, stop producers and timers, wait/join workers as required, detach/remove GUI objects and callbacks before freeing their context, free owned objects and close records exactly once. Follow the target API contracts rather than a universal teardown order.
- GUI callbacks run on the GUI thread in the examined official firmware. Keep them short and nonblocking; move storage, long hardware operations and waits to suitable workers. Use the project's queue or dispatcher for events and API-provided model locking or other synchronization for state shared between threads. Avoid deadlocks, stale callback contexts and unsynchronized pointers.
- Use bounded buffers, safe formatting, correct encoding and explicit error handling for storage and external input. Preserve user data, respect filesystem paths/SD availability, validate file contents before use and avoid leaving hardware or notifications active on exit.

## Verification

1. Build with the intended SDK; include warnings and the exact firmware/tool versions in the report. Run available formatting, static analysis and project tests.
2. Exercise navigation, redraw/update races, failure paths, repeated entry/exit and application cleanup on target firmware/hardware where available. Test peripherals and storage only as appropriate and with user consent.
3. Inspect screenshots and test all layout states using [the verification matrix](layout.md#verification-matrix). A static source inspection, successful compile, emulator or simulation is not proof of real-device behavior.
4. Explain what was observed, what was inferred, target-specific limitations and every untested check. See [sources](sources.md).
