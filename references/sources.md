# Source evidence

Research performed 2026-10-31. Official APIs and behavior below are observations at pinned revisions, not guarantees across all firmware or forks. Community app observations are examples and counterexamples, **not** official rules. Full research records include exact revisions, URLs and limitations.

## Official firmware and tools

- [Official research](../research/official.md): display and status-bar frames; Canvas, fonts and text helpers; UI models and callbacks; lifecycle, FAP manifests, SDK exports, uFBT and tests.
- Firmware dev commit [`9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a`](https://github.com/flipperdevices/flipperzero-firmware/tree/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a); especially [GUI dimensions](https://github.com/flipperdevices/flipperzero-firmware/blob/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a/applications/services/gui/gui_i.h), [layer/frame setup](https://github.com/flipperdevices/flipperzero-firmware/blob/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a/applications/services/gui/gui.c), [Canvas API](https://github.com/flipperdevices/flipperzero-firmware/blob/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a/applications/services/gui/canvas.h), [Canvas implementation](https://github.com/flipperdevices/flipperzero-firmware/blob/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a/applications/services/gui/canvas.c), [fitting helpers](https://github.com/flipperdevices/flipperzero-firmware/blob/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a/applications/services/gui/elements.c), [view model](https://github.com/flipperdevices/flipperzero-firmware/blob/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a/applications/services/gui/view.h) and [viewport callbacks](https://github.com/flipperdevices/flipperzero-firmware/blob/9981d9d4aa5ec7b14bb46cf15763dd293a7dec6a/applications/services/gui/view_port.h).
- [uFBT source](https://github.com/flipperdevices/flipperzero-ufbt/tree/4c603daa164430d791cb58ef019f18721ced7f0d) documents builds using firmware SDKs.

## Community apps

- [UI survey](../research/apps-ui.md): GhostTag, PowerMeter, Trivia Zero and NW Crawl; direct source ranges cover layout regressions, measured fallback, input, row spacing and clipping hazards.
- [Engineering survey](../research/apps-engineering.md): architecture, manifests, APIs, resource ownership, build workflows and tradeoffs from multiple source-reviewed applications.
- [Portability and licensing](../research/portability.md): official agent documentation and Creative Commons sources. Runtime discovery was not tested.

The research records describe the evidence, verification and known gaps.
