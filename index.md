extends: default.liquid
title: Compiling Rust to your Browser
---

## Rust Belt Rust, 2016-10-27

The following is the material used for the [Rust Belt Rust](http://www.rust-belt-rust.com/sessions/) workshop "Hire me for the JS, keep me for the Rust".

* [Slides Talk](slides/rbr-talk)
* [Slides Hands-On](slides/handson/)
* [Demos](demos/)

## Examples

Some bigger examples of Rust code compiled to JavaScript or WebAssembly. Code will be made published soon (but is more or less the default example with some JavaScript wrappers).

* [Calculate SHA1 digest](sha1/)
* [Moving Triangle](triangle/), code: [badboy/rust-triangle-js](https://github.com/badboy/rust-triangle-js)
* [Moving Teapot](teapot/)
* [Factorial in WebAssembly](wasm-fact/)
* [Rhai, an embedded scripting language](rhai-asm/) (asm.js version)
* [Rhai, an embedded scripting language](rhai-wasm/) (WASM version)

## Pre-built compiler

To get a `rustc` with Emscripten support, you can install a nigthly compiler using rustup. Follow these steps:

```
$ rustup toolchain add nightly
$ rustup target add asmjs-unknown-emscripten --toolchain nightly
$ rustup target add wasm32-unknown-emscripten --toolchain nightly
```

You still need the Emscripten SDK, see [how to download and install it in detail](http://kripken.github.io/emscripten-site/docs/getting_started/downloads.html) or follow the simplified steps below (please refer to the official documentation for requirements and installation on Windows):

```
$ wget https://s3.amazonaws.com/mozilla-games/emscripten/releases/emsdk-portable.tar.gz
$ tar -xvf emsdk-portable.tar.gz
$ cd emsdk_portable
$ ./emsdk update
$ ./emsdk install sdk-incoming-64bit
```

Once everything is set up, you can compile with Emscripten:

```
$ rustc --target asmjs-unknown-emscripten hello.rs
```

You can also compile to WebAssembly instead:

```
$ rustc --target wasm32-unknown-emscripten hello.rs
```

## Playground

To test it live in the browser, you can use a custom version of the playground:

[play.hellorust.com](http://play.hellorust.com)

**Caution: It is slow and might fail.**

## Build it yourself

With recent merges to upstream rustc, it became easier to build it yourself. Follow these steps:

```
curl -O https://s3.amazonaws.com/mozilla-games/emscripten/releases/emsdk-portable.tar.gz
tar -xzf emsdk-portable.tar.gz
source emsdk_portable/emsdk_env.sh
emsdk update
emsdk install sdk-incoming-64bit
emsdk activate sdk-incoming-64bit
git clone https://github.com/rust-lang/rust && cd rust
./configure --enable-rustbuild --target=asmjs-unknown-emscripten,wasm32-unknown-emscripten
python2 src/bootstrap/bootstrap.py
echo 'fn main() { println!("Hello, Emscripten!"); }' > hello.rs
build/x86_64-unknown-linux-gnu/stage2/bin/rustc --target=asmjs-unknown-emscripten hello.rs
node hello.js
```

## Resources

* [Compiling Rust to asm.js](https://www.youtube.com/watch?v=bvJCMhJ3RnQ)  
  talk given at [rust.cologne](http://rust.cologne/) in September
* [#36317: Initial webassembly support via LLVM](https://github.com/rust-lang/rust/issues/36317)  
  Tracking issue for getting WASM support into rustc
* [#34743: LLVM upgrade](https://github.com/rust-lang/rust/pull/34743)  
  the PR that started it all
* [Need help with emscripten port](https://internals.rust-lang.org/t/need-help-with-emscripten-port/3154/104)  
  coordinating thread on irlo
