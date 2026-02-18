# Per-Image Sizing and Layout Options — Implementation Plan

## Overview

Extend `WhereRustUsed.vue` so each detail entry can specify a custom image CSS class and choose between a stacked (default) or side-by-side layout.

## Files to Modify

- `components/WhereRustUsed.vue` — data structure and template changes

## Implementation Steps

### Step 1: Extend the `details` data structure

- **Where**: `components/WhereRustUsed.vue`, the `details` object (line 132)
- **What**: Add two optional fields to each entry:
  - `imageClass` (string, optional) — CSS classes for the `<img>` tag. When omitted, fall back to the current default `"h-36"`.
  - `layout` (string, optional) — either `"side"` or omitted/`"stacked"`. When omitted, use the current stacked behavior.
- **How**: For entries that need custom sizing or layout, add the fields directly. Entries that omit them keep existing behavior. Example shape:

```js
linux: {
    text: "...",
    image: "/images/slide6_img3.png",
    imageClass: "h-48",       // optional, defaults to "h-36"
    layout: "side",           // optional, defaults to stacked
},
embedded: {
    text: "...",
    image: null,
    // no imageClass or layout needed — no image
},
```

### Step 2: Update the template for conditional layout

- **Where**: `components/WhereRustUsed.vue`, the right-panel detail block (lines 54-64)
- **What**: Replace the current static `<div>` inside the `<Transition>` with two layout branches based on `currentDetail.layout`.
- **How**: Use a wrapper `<div>` whose flex direction changes based on layout value.

Replace the current detail block (lines 54-64):

```html
<div v-if="currentDetail" :key="focusedItem">
    <p class="text-base leading-relaxed mb-6" v-html="currentDetail.text" />
    <img v-if="currentDetail.image" :src="currentDetail.image" class="h-36 object-contain mx-auto" />
</div>
```

With:

```html
<div
    v-if="currentDetail"
    :key="focusedItem"
    :class="currentDetail.layout === 'side'
        ? 'flex flex-row items-center gap-6'
        : 'flex flex-col'"
>
    <p
        class="text-base leading-relaxed"
        :class="currentDetail.layout === 'side' ? 'flex-1' : 'mb-6'"
        v-html="currentDetail.text"
    />
    <img
        v-if="currentDetail.image"
        :src="currentDetail.image"
        :class="[
            'object-contain',
            currentDetail.layout === 'side' ? '' : 'mx-auto',
            currentDetail.imageClass || 'h-36'
        ]"
    />
</div>
```

Key changes:
- The outer `<div>` switches between `flex-row` (side layout) and `flex-col` (stacked layout).
- In side layout, the text gets `flex-1` to fill remaining space; in stacked layout it keeps `mb-6` for bottom margin.
- The `<img>` uses `currentDetail.imageClass` when provided, otherwise falls back to `"h-36"`. The `mx-auto` centering class only applies in stacked mode.

### Step 3: Apply per-entry values to existing data

- **Where**: `components/WhereRustUsed.vue`, the `details` object
- **What**: Add `imageClass` and/or `layout` to whichever entries need non-default behavior. Leave all other entries untouched — they will use the defaults from the template logic.
- **How**: For example, to make the `android` entry use a larger image in side layout:

```js
android: {
    text: '...',
    image: "/images/slide7_img4.png",
    imageClass: "h-48",
    layout: "side",
},
```

Entries with `image: null` need no changes since the `<img>` is conditionally rendered.
