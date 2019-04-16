---
title: "Docker"
weight: 4
---

You can use our Docker image to be able to run the TinyGo compiler on your computer without having to install all the dependencies. Read on to learn how.

## Installing

    docker pull tinygo/tinygo

## Using

A docker container exists for easy access to the TinyGo CLI. For example, to compile `wasm.wasm` for the WebAssembly example, from the root of the repository:

    docker run --rm -v $(pwd):/src tinygo/tinygo tinygo build -o /src/wasm.wasm -target wasm examples/wasm

To compile `blinky1.hex` targeting an ARM microcontroller, such as the PCA10040:

    docker run --rm -v $(pwd):/src tinygo/tinygo tinygo build -o /src/blinky1.hex -size=short -target=pca10040 examples/blinky1

To compile `blinky1.hex` targeting an AVR microcontroller such as the Arduino:

    docker run --rm -v $(pwd):/src tinygo/tinygo tinygo build -o /src/blinky1.hex -size=short -target=arduino examples/blinky1

For projects that have remote dependencies outside of the standard library and go code within your own project, you will need to map your entire GOPATH into the docker image in order for those dependencies to be found:

    docker run  -v $(PWD):/mysrc -v $GOPATH:/gohost -e "GOPATH=$GOPATH:/gohost" tinygo/tinygo tinygo build -o /mysrc/wasmout.wasm -target wasm /mysrc/wasm-main.go

**note: At this time, tinygo does not resolve dependencies from the /vendor/ folder within your project.**

For microcontroller development you must flash your hardware devices from your host environment, since you cannot run `tinygo flash` from inside the docker container.

So your workflow could be:

- Compile TinyGo code using the Docker container into a HEX file.
- Flash the HEX file from your host environment to the target microcontroller.
