# Introduction

[TODO: Add note about intentional redundancy?]

Hello and welcome to WGPU for Beginners! This book explores the world of WGPU from a low level. If you're looking for a guide that gets you from 0 to triangle, [sotrh's Learn Wgpu](https://sotrh.github.io/learn-wgpu/) book is likely the way to go as it's currently more fleshed out and does exactly that.

This book explores WGPU from a lower level, starting from the ground up. As such, it speaks of WGPU generally and attempts to get less caught up on the minutia of developing an app that only uses WGPU for rendering, although it will include examples of such things. For an example of this, this book, instead of beginning with rendering to a window and going through how to set up such a window and react to events, begins with the GPU itself and focuses on the actual interactions with it. This is mainly conveyed by beginning with GPGPU and GPU computing as this showcases a WGPU use with the least moving parts to examine off the bat.

In short, "WGPU for Beginners" is for beginners to WGPU and more direct GPU interaction, not necessarily for beginners to application building or programming itself. That said, there is still much someone who wants to get into such application development can get out of this book. It has in it several examples and many snippets of code, many of which correspond to actual runnable examples located in the source code of this book under the "ex" subdirectory.

In addition, although WGPU has bindings in C, C++, and other languages to wgpu-native (see [here](https://github.com/gfx-rs/wgpu-native#bindings)), this book will mainly focus on Rust as it is by far the most common language used with WGPU and its incredible support for WASM makes it not only possible but easy to demonstrate WGPU on the web in WASM by being able to run the same function on WASM as native with just a few lines of setup code.

## Navigating This Book

[TODO]

### Footnotes and Reference Links

Always keep a look out for foot notes such as the one attached to this sentence.[^1] This book frequently references the [wgpu crate documentation](https://docs.rs/wgpu/latest/wgpu/index.html), the [WebGPU specification](https://www.w3.org/TR/webgpu/), and the [WGSL specification](https://www.w3.org/TR/WGSL/) (see more information on what these documents are in [What is Wgpu?](./what_is_wgpu)).

## Included Examples

This book's source comes with examples of WGPU being used in Rust which can be found in the "ex/src/bin" subdirectory. Chunks of code from these examples will be featured in this book.

[^1]: Footnotes look like this and the full text/link will appear down here at the bottom of the page which can easily be accessed and linked to by clicking on the little superscript number/text.
