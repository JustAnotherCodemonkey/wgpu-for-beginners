# Using Wgpu: Setup

_"Are we there yet? No; we're just getting our stuff in the car now."_

This chapter describes how to get started with wgpu and the setup that will be assumed for the guide portions of this book.

## Tooling

Since this guide assumes that you are familiar with Rust, it also assumes that you have a functional development environment for Rust, complete with cargo.

## Target Requirements

A table of supported platforms and the officially supported backends for each one can be found under the [Supported Platforms section of the wgpu official repo readme](https://github.com/gfx-rs/wgpu/tree/trunk?tab=readme-ov-file#supported-platforms).

```admonish tldr
This section says, in many words, that any native machine you run a program using wgpu on needs to have 3 things:

1. The machine's OS is supported
1. The machine has at least 1 wgpu-supported backend available
1. At least one backend supported by both the OS and wgpu can identify and aquire a handle to a graphics device on the machine

This describes virtually all modern personal computers which run Windows, Linux, or macOS, support at least one wgpu-compatible backend, and have an integrated graphics processor if not a discrete graphics card.

On the web, everything is handled by the browser and is taken care of. Note that WebGPU is still not supported by most browsers but WebGL is practically guarenteed to be present and to be able to get a device.

If you don't need details, you can skip this section. This section is being considered for simplification.
```

Assuming the target you are compiling for (which is probably your own computer) is supported, you will need, for your platform, at least one supported backend installed in order to create an `Instance` at runtime, the ultimate originator of all functionality within wgpu. 

```admonish note title="Note for Some Users"
To have a backend "installed", the relevant system libraries must be installed such that plain calls to the OS to open them (const-defined path to `dlopen`, etc.) will succeed.

This is mainly for users of minimalist Linux distros (Arch Linux, Void Linux, etc.); other OS's and most distros will have all supported graphics libraries installed for you.

If you want to use a custom set of `.so`'s for your backend lib sources for hacking, you will need to use external methods such as the `LD_LIBRARY_PATH` or `DT_RPATH` ELF tag on Linux (see <https://man7.org/linux/man-pages/man3/dlopen.3.html>).

This is irrelevant for WASH since all calls are directly passed to the browser which then decides how to handle them.
```

Obviously, you will not just want to create an `Instance`. Locating graphics devices is a task delegated to the backend library and thus what hardware counts as a "device" is decided by the native backend. On WASM, this can be particularly tricky as all calls are delegated to the browser which then executes them using its own backend of choice. Luckily though, all the native backends tend to be very good about being inclusive to a variety of devices from discrete NVIDIA graphics cards to integrated ARM GPUs.

```admonish note
Note that just because the target has a device that wgpu will be able to access does not mean that you will always be able to use it. To abstract another chapter's section [TODO: Link the chapter#section that talks about Limits and Features], you may need your device to have certain capabilities for your code to work. However, not all backends and not all devices support all the capabilities you can request in wgpu. This is especially true about computing which is not supported when the GL backend uses GLES <=3.0 or WebGL2. This can be a difficult problem to debug.
```

## Setting Up a Project

When compiling, wgpu is no different from any other Rust dependency. Simply add it to your crate's `Cargo.toml` or use `cargo add wgpu` to easily add the latest version to your crate dependencies.

## (Optional) Set Up Toolchain for Web Deployment

If you want to test or otherwise deploy your code for running in WASM, you need to have the wasm32-unknown-unknown target installed. If you're using `rustup` as your Rust toolchain manager, this is as easy as `rustup toolcain install wasm32-unknown-unknown`.

For those using other systems, you will likely need to consult the relevant documentation for your package manager for how to do this. 

```admonish tip title="For NixOS Users"
If you're using NixOS and you haven't already, you should really consider [oxalica's rust-overlay](https://github.com/oxalica/rust-overlay) as it lets you easily manage targets and select your preferred toolchain altogether, including nightly Rust.
```

## Time to Code

That should be all you need to get up and running with the rest of this book.
