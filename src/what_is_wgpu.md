# What is Wgpu?

## What is WebGPU?

To understand wgpu is to understand WebGPU or at least what it is. For the uninitiated, WebGPU is, in the words of its own specification, "an API that exposes the capabilities of GPU hardware for the Web."[^1] The "GPU hardware" in question can refer to any graphics processing hardware, including integrated graphics processors as well as discrete graphics cards, on the user's machine, regardless of it's vendor/model-specific advanced capabilities (such as CUDA or OptiX) or native API capabilities (discussed below).

WebGPU, in its goal, is also meant to be OS-agnostic, which is significant because not all operating systems support all graphics APIs; DirectX is for Windows, Metal is specific to MacOS, and MacOS specifically exclusively supports Metal.

In short, WebGPU is meant to provide GPU access to any JavaScript/WASM executor (like one found in a browser or Node) that supports it, regardless of platform or hardware. If a user's machine has hardware that can render to the screen, a complete implementation of WebGPU can use it to render to a web canvas or compute on it too.

### Differences with WebGL

As the spec says, "WebGPU is not related to WebGL and does not explicitly target OpenGL ES."[^1]. The API for WebGPU is very different. [TODO: Identify some key specific API differences.]

Additionally, unlike WebGL, which uses the GLSL shading language to define shaders to mirror OpenGL, WebGPU uses its own modernized shading language known as WGSL (specification can be found at [w3.org/TR/WGSL](https://www.w3.org/TR/WGSL/#intro)) which is significantly different in a number of ways from GLSL.

## What is Wgpu?

Wgpu is a cross-platform library with an API that mirrors WebGPU, translating WebGPU API calls to native, platform-supported graphics APIs.

Think of it like this: A WebGPU-supporting browser loads a website that uses JavaScript or a WASM module to, say, render a triangle to a canvas. The JS/WASM makes API calls to WebGPU. The browser now has the task of correctly executing those API calls on its machine. Luckily, WebGPU is developed with compatibility with native graphics APIs in mind. That is, it should be possible to translate all WebGPU API calls into native API calls with minimal emulation of functionality with the benefits of both ease of implementation as well as performance. The bad news is that this is still an incredibly difficult task in of itself, involving thousands of lines of code and hundreds of hours to correctly implement for all the platforms you expect to deploy your JavaScript-executing application to.

This is where wgpu comes in. Wgpu is that implementation, presenting a friendly, WebGPU-like API that can easily be translated to from WebGPU calls. Currently, Firefox is an example of such a cross-platform application that uses wgpu to handle and execute the WebGPU calls it handles from JavaScript/WASM, with potentially more large browsers that are planning to support WebGPU, using wgpu in the future.

In reality though, wgpu is much more than that. Its ability to provide cross-platform, hardware-independent GPU access with APIs in multiple languages makes it highly desirable on its own, independent of WebGPU. Currently, wgpu is by far the most popular cross-platform graphics library in Rust with nearly 4 million downloads on [crates.io](https://crates.io/), putting it in 4th place for all-time downloads under the "graphics" tag, with places 1, 2, and 4 all going to dependencies of the wgpu crate.[^2]

## Wgpu's Capabilities and Limitations

### Platform Support

As it says on the landing page of wgpu's website, [wgpu.rs](https://wgpu.rs/), "Applications using wgpu run natively on Vulkan, Metal, DirectX 12, and OpenGL ES; and browsers via WebAssembly on WebGPU and WebGL2."

Yes, wgpu, when compiled to WASM, makes API calls to WebGPU or WebGL, leading to the potential for a program using wgpu for GPU access from WASM to end up making WebGPU API calls to a browser which itself uses wgpu under the hood to handle those API calls.

### Shading Language Support

Because wgpu is not WebGPU, even though it may work by ultimately making calls to WebGPU, it still has the ability to add certain functionality.

Specifically, in order to process the WGSL modules it receives either from native usage or being used as a backend to a JS/WASM executor, wgpu uses [naga](https://crates.io/crates/naga/0.19.2), the universal shader translator, which was recently completely abducted into the wgpu project itself. Naga is capable of taking in WGSL, GLSL, or SPIR-V and parsing them into its own IR (Intermediate Representation). It can then output SPIR-V, WGSL, Metal, HLSL, GLSL, or DOT shader code.

This allows wgpu to parse WGSL into backend-specific code that can then be passed to the relevant API. It also means, however, that when used as a library, in addition to taking WGSL, it can also take in GLSL; SPIR-V bytecode, allowing you to program in any language that compiles to SPIR-V; or even naga IR itself, allowing you to define shaders programmatically or bring your own parser for your own shader language. [TODO: Add examples that use these other languages and link them here]

### Limitations

Unfortunately, cross-platform API's always come at a cost: in order to be able to target many platforms, you are untimately, to some degree, fundamentally limited by these platforms. If a single platform doesn't support a feature that all the others do, you cannot include it in your public API. Wgpu attempts to reduce the severity of this limitation by enabling the user to, when attempting to acquire a handle to a device, request that the device support specific hardware/native-API-dependant additional features.

Being able to request additional potentially unsupported features is also a part of WebGPU as is outlined in the spec.[^3] However, wgpu's selection of optional features goes far beyond this with over 40 total optional features, very many of which are not available when using WebGPU, still many not available when compiling to WASM at all.[^4]

[^1]: <https://www.w3.org/TR/webgpu/#intro>
[^2]: <https://crates.io/keywords/graphics?sort=downloads>
[^3]: <https://www.w3.org/TR/webgpu/#features>; a list of all available features in WebGPU can be found here: <https://www.w3.org/TR/webgpu/#feature-index>
[^4]: <https://docs.rs/wgpu/latest/wgpu/struct.Features.html#>
