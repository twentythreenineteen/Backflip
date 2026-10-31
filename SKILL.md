---
name: backflip
description: Designs, implements, reviews, and tests Flipper Zero FAP applications. Use for FAP UI, Canvas layout, scene/view navigation, firmware APIs, manifests, assets, and builds; prioritize pixel bounds and non-overlapping elements.
---

# Backflip: Flipper Zero FAP development

## Workflow

1. Inspect the target project's existing manifest, app entry point, UI patterns, assets, tests, firmware/SDK and build commands. Reuse its established conventions; identify firmware version and supported public exports before choosing APIs.
2. Select the smallest appropriate firmware UI module; use custom Canvas drawing only when needed. Map screens, states, Back/OK/directional and short/long/repeat input; make errors and empty/loading states navigable.
3. For every custom screen, follow [pixel-safe layout](references/layout.md): identify the actual local canvas, allocate non-overlapping rectangles, measure text and icons, and verify dynamic and extreme values. A 128×64 physical display does not imply every viewport is 128×64.
4. Follow [FAP engineering](references/engineering.md) for manifest, SDK compatibility, callbacks, state ownership, resources and teardown. Prefer existing public APIs and app patterns; consider failures and cleanup paths.
5. Build against the intended SDK and exercise the app on the intended firmware, preferably on hardware. Review every screen and state, interactions, Back/exit, longest content, slow/error paths and repeated entry/exit. Capture screenshots when available. Distinguish static geometry checks from a visual test; report untested cases and constraints.
6. Consult [source evidence](references/sources.md) when a convention or version-specific detail needs confirmation. User requirements and actual target SDK take precedence over generic examples.

## Anti-Patterns

- Assume canvas frame bounds clip drawing, or apply screen coordinates to a window-local canvas.
- Place strings by guessed character count, ignore font baseline/descenders, or let dynamic content collide with controls.
- Block the GUI thread with storage, waits, work loops or hardware transactions; mutate shared state without its required synchronization.
- Present an unbuilt FAP, static calculations or an emulator screenshot as hardware verification.
