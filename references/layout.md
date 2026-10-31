# Pixel-safe Canvas layout

Read this for every custom-drawn screen, or when reviewing overlap, clipping, scrolling or text placement. Values below describe horizontal orientation; verify the target firmware and actual canvas, including rotation.

## Establish the coordinate space

- Physical horizontal screen: 128×64 pixels; valid physical coordinates are x=0..127 and y=0..63. Rectangles use `[x, x+w) × [y, y+h)` throughout this guide: `w` and `h` are counts, not last coordinates.
- In the examined official firmware, fullscreen uses a 128×64 canvas. A window below the status bar uses a 128×51 canvas with local (0,0) at physical (0,13). Drawing in local coordinates is already offset; do not add 13 again. This is a version-specific observation, not a promise for every firmware or orientation.
- During draw, use `canvas_width(canvas)` and `canvas_height(canvas)` as the actual local frame dimensions. Determine the GUI layer and orientation explicitly; account for any parent/module header, footer or icon margin within that frame.
- In the examined implementation, frame setup changes origin and reported size; it is not a general clip rectangle. Do not depend on it to protect the status bar, adjacent regions or the physical edge. Check the primitives and renderer in the targeted SDK.

## Make a layout budget before coding

1. Write each occupied region as `(x, y, width, height)` in local coordinates. Name its owner and state: header, body rows, borders, scrolling indicator and button hints. Include padding, stroke extent and the actual icon dimensions.
2. Require `x >= 0`, `y >= 0`, `w > 0`, `h > 0`, `x + w <= canvas_width`, `y + h <= canvas_height`. Perform arithmetic in a signed, sufficiently wide type before casting to API coordinate types; avoid unsigned underflow. Different mutually exclusive states may reuse a rectangle; simultaneously visible elements may not overlap except when intentional and legible (e.g. selection background behind its text).
3. Check every pair of simultaneously visible bounding boxes. They overlap iff `a.x < b.x+b.w && b.x < a.x+a.w && a.y < b.y+b.h && b.y < a.y+a.h`. Include 1px-or-larger explicit gaps where needed, not imaginary inclusive endpoints. For lines and frames, budget all drawn pixels, including endpoints.
4. Reserve fixed regions first; give the remainder to variable content. For `n` equal rows starting at `y`, require `y + n*row_height <= body_bottom` (exclusive). Scroll or paginate when the list does not fit; keep selection and indicators within their reserved regions.

**Worked budget:** fullscreen 128×64, header `[0,16)`, four rows of height 12 starting at 16 → body `[16,64)`; the final row is `[52,64)`, ending at y=63. Four 13px rows instead end at y=67 and overflow. This proves only the row boxes, not that a font, icon, selection frame or footer fits inside them. A 128×51 window needs a separate budget.

## Text and assets

- Set the intended font first; use `canvas_string_width` on the **actual rendered** string, not character count. Use `canvas_current_font_height` and target font parameters as planning aids, not an exact bounding box for every glyph. `canvas_draw_str` uses a baseline; aligned drawing uses its documented anchor and ascent, so ascenders, descenders and border padding still need space.
- Width is not automatically constrained. For dynamic names/values, reserve pixels and choose an explicit policy: truncate with a measured ellipsis, wrap at valid boundaries, scroll, paginate, or use an existing text module. Measure the resulting displayed string again, including suffix; specify a fallback if even the ellipsis cannot fit. Truncate UTF-8 only at code-point boundaries; test unavailable glyphs and locale-dependent lengths. Do not mutate a shared source string merely to fit a draw. The examined `elements_string_fit_width` mutates its input, and its implementation warrants tests for tiny widths and multibyte truncation; verify target behavior before use.
- Reserve the icon's real width/height (including transparent pixels if relevant), the text's rendered extent and a gap. Center/right alignment is an anchor, not protection from overflow. Controls at the bottom require room for descenders and borders. Avoid relying on `AlignBottom` as glyph clipping.
- Preserve contrast between selection/background and text/icons; monochrome screenshots reveal failures that coordinates alone cannot.

## Verification matrix

- Check all ordinary and empty/loading/error/confirmation screens, every selection, Back path, first/last scroll pages, buttons and overlays. Draw conditional elements simultaneously in the combinations that can actually occur.
- Exercise shortest/longest realistic labels, UTF-8/localized text if supported, missing glyphs, maximum numbers and units, zero/one/many items, and any asynchronous value update. Check status-bar and fullscreen modes as applicable.
- Add simple layout assertions/unit tests where possible for rectangles and pairwise collisions; calculate boundary cases such as x+w exactly equal to width. Such checks cannot prove glyph ink, clipping or contrast. Capture and inspect screenshots on target firmware/device (or emulator if available); report which were actually observed. Iterate until no unintended overlap or edge overflow remains.

See [source evidence](sources.md) for pinned firmware and app examples.
