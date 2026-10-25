# UI prototype

Several structurally different variations on one route, switchable from a floating bar. The user flips through, picks one or combines parts, and the rest is discarded.

## Where

- **Preferred: inside an existing page.** Render variants on the real route, gated by `?variant=`. Keep the page's real data fetching, auth, header, and sidebar; only the subtree being decided swaps. A variant judged next to the real app exposes problems an empty page hides. Something that would naturally live inside a page (a new section, card, or step) still goes here.
- **Last resort: a new throwaway route**, only when nothing existing could host it. Follow the project's routing convention; put `prototype` in the path.

## Build

1. Write the plan in one line at the top of the switcher file: "Three variants of <X>, switchable via `?variant=`, on `<route>`." Default 3 variants, max 5.
2. Each variant uses the project's real component library and styling, and is exported with a clear name (`VariantA` ... or a descriptive one).
3. Variants must differ in layout, information hierarchy, or primary affordance. Share small leaf components (a header) if useful; never share the layout.
4. Mutations point at stubs. The question is how it looks, not whether the backend works.

## Switcher

A small fixed pill at the bottom center, visually distinct from the design being judged:
- left arrow / current label (`B - Sidebar layout`) / right arrow; wraps around
- updates the URL param through the framework's router, so links are shareable and reload-stable
- left/right arrow keys cycle, except while focus is in an input, textarea, or contenteditable
- rendered only outside production builds (e.g. `NODE_ENV !== 'production'`)

## Observe

Screenshot every variant at a realistic viewport, with realistic data, and drive the main interaction in each. Report the URL and the variant keys.

## Clean up after the decision

Fold the winner into the real page by rebuilding it properly (variant code was written without tests). Remove losing variants and the switcher from the working branch; keep them on the `prototype/<name>` branch.
