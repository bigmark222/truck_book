# Overview

Welcome to the Truck world!
Truck is a pure-Rust CAD kernel that aims to provide modern, safe, and highly modular tools for geometric modeling.
This book will guide you through the basics of using Truck.

The Three Core Ideas Behind Truck

Truck’s design philosophy revolves around three complementary elements:

1. Trendy Tools

Truck is built with Rust and WebGPU, two technologies pushing the next generation of performance-focused engineering software.

Rust gives strong memory safety and powerful optimizations.

WebGPU allows fast, cross-platform graphics and computational workloads.

Together, they help Truck’s crates achieve high performance while remaining safe and maintainable.

2. Traditional Arts (Modernized)

Truck re-implements classic CAD concepts—such as B-rep and NURBS—but in a modern environment using Rust and WebGPU.

Rust prevents crashes that commonly happen in low-level CAD code.

Cargo and the Rust ecosystem make continuous integration and testing much smoother.

The goal is to take the best ideas from decades of CAD history but build them with a clean, modern foundation.

3. Theseus’ Ship (Modular Design)

Inspired by the “Ship of Theseus,” Truck is divided into small, replaceable crates instead of one giant system.

Why?

Individual crates can evolve independently.

You can replace or upgrade only the pieces you need.

Complex expansions won’t break the entire system.

This modular structure keeps Truck flexible and future-proof.

Structure of This Book

To help different types of users learn effectively, we consider three levels of involvement:

Users

People who want to build applications using Truck and other libraries.

Developers

People who create new geometric elements or tools on top of Truck and publish them for others.

Contributors

People who contribute directly to Truck’s codebase through pull requests.

This tutorial focuses mainly on Users who want to learn how to work with Truck and make things using it.

Sample Code

All full code examples from this tutorial are available in this repository:

https://github.com/ricosjp/truck-tutorial-code/tree/v0.1

For example:

The file src/section2_1.rs corresponds to Section 2.1 (“First Triangle”).

You can run it with:

cargo run --bin section2_1

System Requirements

To use Truck, you need:

A working Rust development environment.

A GPU backend supported by WebGPU: Vulkan, Metal, or DirectX12.

CMake (only required for running tests; not required for using Truck as a library).

Setup instructions differ by platform:

Windows

Steps:

Make sure Windows 10 is up to date.

Install “Visual Studio C++ Build Tools.”
If already installed, update them.

Install Rust by using the official installer:
https://www.rust-lang.org/tools/install

Choose the MSVC toolchain.
If Rust is already installed, run rustup update.

Notes:

Truck is tested only with the MSVC Rust toolchain.

Vulkan and DirectX12 should work on an up-to-date Windows 10 system.

Truck does not run correctly on WSL due to missing Vulkan support.

Windows 8 is likely unsupported.

macOS

Steps:

Make sure macOS is up to date.

Install Rust using:

curl https://sh.rustup.rs -sSf | sh


If Rust is already installed, run:

rustup update


Notes:

Metal (Apple’s GPU API) works out of the box.

No additional GPU setup is needed.

Linux

Linux is currently not officially supported because Truck hasn't been tested thoroughly on Linux native environments.

You may try installing Vulkan manually.
A helpful reference:
https://vulkan.lunarg.com/doc/sdk/1.2.162.1/linux/getting_started_ubuntu.html

Testing inside official Docker containers did not work.

CI builds currently use:
nvidia/vulkan (Docker Hub)