# MuPDF.js

TEMPORARY FORK FOR VIVLIOSTYLE CLI

This fork provides a prerelease build of the MuPDF.js for Vivliostyle CLI
(https://github.com/vivliostyle/vivliostyle-cli).

This prerelease exists mainly to pick up the following fixes before they
appear in an official MuPDF release:

- https://bugs.ghostscript.com/show_bug.cgi?id=709796
  (already on the 1.28.x branch, but not yet in a release)
- https://bugs.ghostscript.com/show_bug.cgi?id=709802
- https://bugs.ghostscript.com/show_bug.cgi?id=709930

## Development

### Clone

MuPDF's `.gitmodules` uses relative URLs for submodules. When cloning from this
fork, the URLs resolve to `u1f992/*` repositories that do not exist. Point the
remote to the upstream repository before initializing submodules:

```sh
git clone https://github.com/u1f992/mupdf.git --branch 1.29.0-alpha.0
cd mupdf
git remote set-url origin https://github.com/ArtifexSoftware/mupdf.git
git submodule sync --recursive
git submodule update --init --recursive --depth 1
cd platform/wasm
```

### Build

Build the WASM package using Docker (emsdk 4.0.8, as specified in
`BUILDING.md`):

```sh
docker pull emscripten/emsdk:4.0.8
docker run --rm --volume ../..:/src --workdir /src/platform/wasm emscripten/emsdk:4.0.8 \
  bash -c 'apt-get update --quiet=2 && apt-get install --yes --quiet=2 brotli > /dev/null 2>&1 && npm install && EMSDK=/emsdk npm run prepack'
docker run --rm --volume ../..:/src emscripten/emsdk:4.0.8 chown --recursive "$(id -u):$(id -g)" /src
```

The built package is in `dist/`.

### Pack and publish

Use `--ignore-scripts` to skip the `prepack` script (build + compress), which
has already been run inside Docker:

```sh
npm pack --ignore-scripts
```

A prerelease version requires an explicit dist-tag so that it does not become
`latest`. Check the package contents and the resulting name, version and tag
with a dry run:

```sh
npm publish --dry-run --ignore-scripts --tag alpha --access public
```

Then publish:

```sh
npm publish --ignore-scripts --tag alpha --access public
```

<details>
<summary>Upstream README</summary>

# MuPDF.js

Welcome to the official MuPDF.js library from [Artifex](https://artifex.com).

Use [MuPDF](https://mupdf.com) in your JavaScript and TypeScript projects!

This library is powered by WebAssembly and can be used in all the usual
JavaScript environments: Node, Bun, Firefox, Safari, Chrome, etc.

## License

MuPDF.js is available under Open Source
[AGPL](https://www.gnu.org/licenses/agpl-3.0.html) and commercial license
agreements.

> If you cannot meet the requirements of the AGPL, please contact
> [Artifex](https://artifex.com/contact/mupdf-js) regarding a
> commercial license.

## Installation

	npm install mupdf

## Usage

The module is only available as an ESM module!

	import mupdf from "mupdf"

	var doc = mupdf.Document.openDocument("test.pdf")
	console.log(doc.countPages())

Check out the example projects to help you get started:

- [github.com/ArtifexSoftware/mupdf/tree/master/platform/wasm/examples](https://github.com/ArtifexSoftware/mupdf/tree/master/platform/wasm/examples)
- [github.com/ArtifexSoftware/mupdf.js/tree/master/examples](https://github.com/ArtifexSoftware/mupdf.js/tree/master/examples)

## Documentation

- [MuPDF.js Reference](https://mupdf.readthedocs.io/en/latest/reference/javascript/)
- [Getting Started & Examples](https://mupdfjs.readthedocs.io/en/latest/)

## Contact

Join the Discord at [#mupdf.js](https://discord.gg/zpyAHM7XtF) to chat with the
developers directly.

</details>
