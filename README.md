# permitter

![Preview](./resources/preview.gif)

write licenses to stdout

a fork of [licensor](https://github.com/raftario/licensor)

[![GitHub Workflow](https://img.shields.io/github/actions/workflow/status/adamperkowski/permitter/build.yml)](https://github.com/adamperkowski/permitter/actions?workflowID=build) [![crates.io](https://img.shields.io/crates/v/permitter?color=orange)](https://crates.io/crates/permitter) [![AUR](https://img.shields.io/aur/version/permitter)](https://aur.archlinux.org/packages/permitter)

## About

Write a license to standard output given its SPDX ID. A name for the copyright holder can optionally be provided for licenses where it is included. If the provided ID isn't found, similar ones will be suggested. Licenses are all compiled into the binary.

## Features

* Simple
* Licenses are taken directly from SPDX, no slightly edited templates
* Single binary, licenses are included into it at compile time
* `WITH` exception expressions support
* Ability to skip the copyright notice, which is allowed by the Berne convention
* Adding new licenses just requires editing a JSON file

## Why

I just got tired of losing time looking for license files for my new projects.

## Usage

Here are a couple usage examples to get a general idea of how it all works. For detailed usage, just pass the `--help` flag.

Write the MIT license with a copyright notice to `LICENSE`:

```sh
permitter MIT "Adam Perkowski" > LICENSE
```

Write the Apache 2.0 license with the LLVM exception to `LICENSE`, skipping optional parts:

```sh
permitter "Apache-2.0 WITH LLVM-exception" --skip-optional > LICENSE
```

Print the BSD 3 Clause license without a copyright notice:

```sh
permitter BSD-3-Clause
```

List available licenses
```sh
permitter --licenses
```

## Installation

There are a few installation option available.

You are welcome to distribute this software on other platforms, don't hesitate to open a PR to update this section if you do so!

### [Releases](https://github.com/adamperkowski/permitter/releases/latest)

Pre-compiled binaries of `permitter` can be downloaded from the release page.

### [Crates.io](https://crates.io/crates/permitter)

```sh
cargo install permitter
```

### [Arch Linux](https://aur.archlinux.org/packages/permitter)

```sh
git clone https://aur.archlinux.org/permitter.git
cd permitter
makepkg -si
```

If you use an AUR Helper ([paru](https://github.com/Morganamilo/paru) for example), it's even simplier:
```sh
paru -S permitter
```

## Available licenses and exceptions

See [list](./LIST.md).

## Contributing

Contributors are welcome. If you see anything that could be fixed/improved or have a new feature idea, just open an issue or a PR!

However, try to keep the main CLI as simple and light as possible. Features such as adding licenses/exceptions at runtime or validating licenses are not planned and will not be added.

### Adding licenses

If you'd like a license to be added to the list, you can either open an issue for it or add it yourself, which is fairly easy.

To add a license, just add it to [`resources/licenses.json`](./resources/licenses.json) following the schema provided in [`resources/licenses-schema.json`](./resources/licenses-schema.json). To apply the changes to the main CLI, you'll also need to run both `cargo run -p permitter_fetch` and `cargo run -p permitter_codegen`, in that order. To get the information you need, just refer to the [SPDX License List](https://github.com/spdx/license-list-data).

The same goes for exceptions.

## How it works

First, licenses and exceptions specified in the resources files are parsed from the [SPDX License List](https://github.com/spdx/license-list-data), then gzipped, using the [permitter_fetch](./permitter_fetch) subcrate. Then the [codegen.rs](./src/codegen.rs) file is automatically generated based on the parsed licenses, using the [permitter_codegen](./permitter_codegen) subcrate, and included in the CLI.

Finally, the main CLI is built on its own, which makes the build time relatively fast for end users and only requires dependencies of the main CLI and not ones required by helper subcrates.

## Credits

Thanks to the amazing people on the [/r/rust](https://reddit.com/r/rust) subreddit who provided great feedback and hints, and showed way more enthusiasm than initially expected.

## Licensing

`permitter` is licensed under the [MIT License](./LICENSE).
