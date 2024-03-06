# A Word on Documentation

_"Ok explain it to me like I'm 55."_

References and documentation in a side window/monitor are, to the most experienced developer, what a teddy bear in an arm is to a child in the dark; separation will cause extreme anxiety and sometimes borderline non-function.

The first step of learning to use any library or other piece of software should always be identifying where to find the manual. For most intermediate wgpu users, developing directly with wgpu in Rust is often a 4-browser-tab experience:

1. The Rust std (obviously)
1. The wgpu crate documentation (<https://docs.rs/wgpu/latest/wgpu/index.html>)
1. The WebGPU specification (<https://www.w3.org/TR/webgpu/>)
1. The WGSL specification (<https://www.w3.org/TR/WGSL/>) (if you're working at all with shaders)

With a potential 5th for beginners, that being the examples subdirectory of the wgpu repository (<https://github.com/gfx-rs/wgpu/tree/trunk/examples>), which contains a growing list of official examples of both compute and render pipelines as well as spotlights for some of the more obtuse features of wgpu.

## Navigating the Wgpu Crate Documentation

When it comes to the wgpu crate, there isn't much navigation to do. Most everything (the general API of wgpu itself) is dumped straight in the root of the crate. This is helpful as it allows users to avoid endless lists of imports in lieu of a short in-place path specification whenever a wgpu type is called for, i.e.

```rust,ignore
let i = wgpu::Instance::default();
// ...
```

instead of

```rust,ignore
use wgpu::{Instance, /* A bajillion other imports */};

let i = Instance::default();
// ...
```

It does, however, also make the crate itself difficult to navigate and know where to get started if you don't know what you're looking for. Thats what resources like this one are for.

In addition, the crate docs rarely go into much detail at all on the crate items, instead, keeping it short and linking to the part of the WebGPU spec where its counterpart item is mainly defined.

```admonish tip
The following is a highly reasonable and effective workflow for finding documentation on a known item:

1. Use the search bar to open the crate docs for the item. Always remember that although wgpu's API is heavily based on WebGPU, it is often not a perfect 1:1 mapping and wgpu's items often have additional functionality not explicitly described by WebGPU. There is also of course the fact that the WebGPU spec describes a JavaScript API, not a Rust one. Rust is not JavaScript and any even mapped API will inevitably function different and may have additional helper items.
1. Read the documentation there, especially any caveats to mapping it to any corresponding WebGPU item if it has one.
1. If the documentation there isn't satisfactory and it does have a corresponding WebGPU item linked in the docs, click the link to open the WebGPU spec to the relevant section and skim it for important details.
1. If that still doesn't satisfy you (not too uncommon when searching to answer questions about behavior), continue reading the spec and any relevant sections. Remember to click on any links in the item's section, go back to the table of contents to identify other potentially relevant sections, and use the Ctrl-f feature embedded in all modern browsers to do a text search for the item and relevant keywords. The following section, [Navigating the WebGPU Specification](#navigating_the_webgpu_specification), is your friend on this.
1. If you still need help, consider reaching out as described in the chapter [Getting Help](./getting_help.md).
```

## Navigating the WebGPU and WGSL Specifications

The WebGPU specification is enormous. It has, at time of writing, 26 major chapters, each having between 1 and 23 sub-chapters.[^1]

Luckily, one can generally get away with a couple to start, using others for reference when needed. This book will reference the WebGPU spec as a reference frequently both for sourcing further details as well as giving the reader a mental map of where things can be found in the specification.

In short, how to navigate the WebGPU spec? In a way, that's what this whole book is; a packaged and beginner-friendly tour guide to this spec as well as the one for WGSL.

```admonish tip
Keep a look out for the little citation-like footnote numbers; clicking them will link to the footnotes section at the bottom of the page which often contains links to the relevant/sourced sections in the specs.
```

[^1]: <https://www.w3.org/TR/webgpu/#toc>
