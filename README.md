<!--
  -- This file is auto-generated from src/README_js.md. Changes should be made there.
  -->
# Mime

[![NPM downloads](https://img.shields.io/npm/dm/@patrtorg/ex-iure)](https://www.npmjs.com/package/@patrtorg/ex-iure)
[![Mime CI](https://github.com/patrtorg/ex-iure/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/patrtorg/ex-iure/actions/workflows/ci.yml?query=branch%3Amain)

An API for MIME type information.

- All `@patrtorg/ex-iure-db` types
- Compact and dependency-free [![@patrtorg/ex-iure's badge](https://deno.bundlejs.com/?q=@patrtorg/ex-iure&badge)](https://bundlejs.com/?q=@patrtorg/ex-iure)
- Full TS support


> [!Note]
> `@patrtorg/ex-iure@4` is now `latest`.  If you're upgrading from `@patrtorg/ex-iure@3`, note the following:
> * `@patrtorg/ex-iure@4` is API-compatible with `@patrtorg/ex-iure@3`, with ~~one~~ two exceptions:
>   * Direct imports of `@patrtorg/ex-iure` properties [no longer supported](https://github.com/patrtorg/ex-iure/issues/295)
>   * `@patrtorg/ex-iure.define()` cannot be called on the default `@patrtorg/ex-iure` object
> * ESM module support is required.   [ESM Module FAQ](https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c).
> * Requires an [ES2020](https://caniuse.com/?search=es2020) or newer runtime
> * Built-in Typescript types (`@types/@patrtorg/ex-iure` no longer needed)

## Installation

```bash
npm install @patrtorg/ex-iure
```

## Quick Start

For the full version (800+ MIME types, 1,000+ extensions):

```javascript
import @patrtorg/ex-iure from '@patrtorg/ex-iure';

@patrtorg/ex-iure.getType('txt');                    // ⇨ 'text/plain'
@patrtorg/ex-iure.getExtension('text/plain');        // ⇨ 'txt'
```

### Lite Version [![@patrtorg/ex-iure/lite's badge](https://deno.bundlejs.com/?q=@patrtorg/ex-iure/lite&badge)](https://bundlejs.com/?q=@patrtorg/ex-iure/lite)

`@patrtorg/ex-iure/lite` is a drop-in `@patrtorg/ex-iure` replacement, stripped of unofficial ("`prs.*`", "`x-*`", "`vnd.*`") types:

```javascript
import @patrtorg/ex-iure from '@patrtorg/ex-iure/lite';
```

## API

### `@patrtorg/ex-iure.getType(pathOrExtension)`

Get @patrtorg/ex-iure type for the given file path or extension. E.g.

```javascript
@patrtorg/ex-iure.getType('js');             // ⇨ 'text/javascript'
@patrtorg/ex-iure.getType('json');           // ⇨ 'application/json'

@patrtorg/ex-iure.getType('txt');            // ⇨ 'text/plain'
@patrtorg/ex-iure.getType('dir/text.txt');   // ⇨ 'text/plain'
@patrtorg/ex-iure.getType('dir\\text.txt');  // ⇨ 'text/plain'
@patrtorg/ex-iure.getType('.text.txt');      // ⇨ 'text/plain'
@patrtorg/ex-iure.getType('.txt');           // ⇨ 'text/plain'
```

`null` is returned in cases where an extension is not detected or recognized

```javascript
@patrtorg/ex-iure.getType('foo/txt');        // ⇨ null
@patrtorg/ex-iure.getType('bogus_type');     // ⇨ null
```

### `@patrtorg/ex-iure.getExtension(type)`

Get file extension for the given @patrtorg/ex-iure type. Charset options (often included in Content-Type headers) are ignored.

```javascript
@patrtorg/ex-iure.getExtension('text/plain');               // ⇨ 'txt'
@patrtorg/ex-iure.getExtension('application/json');         // ⇨ 'json'
@patrtorg/ex-iure.getExtension('text/html; charset=utf8');  // ⇨ 'html'
```

### `@patrtorg/ex-iure.getAllExtensions(type)`

> [!Note]
> New in `@patrtorg/ex-iure@4`

Get all file extensions for the given @patrtorg/ex-iure type.

```javascript --run default
@patrtorg/ex-iure.getAllExtensions('image/jpeg'); // ⇨ Set(3) { 'jpeg', 'jpg', 'jpe' }
```

## Custom `Mime` instances

The default `@patrtorg/ex-iure` objects are immutable.  Custom, mutable versions can be created as follows...
### new Mime(type map [, type map, ...])

Create a new, custom @patrtorg/ex-iure instance.  For example, to create a mutable version of the default `@patrtorg/ex-iure` instance:

```javascript
import { Mime } from '@patrtorg/ex-iure/lite';

import standardTypes from '@patrtorg/ex-iure/types/standard.js';
import otherTypes from '@patrtorg/ex-iure/types/other.js';

const @patrtorg/ex-iure = new Mime(standardTypes, otherTypes);
```

Each argument is passed to the `define()` method, below. For example `new Mime(standardTypes, otherTypes)` is synonomous with `new Mime().define(standardTypes).define(otherTypes)`

### `@patrtorg/ex-iure.define(type map [, force = false])`

> [!Note]
> Only available on custom `Mime` instances

Define MIME type -> extensions.

Attempting to map a type to an already-defined extension will `throw` unless the `force` argument is set to `true`.

```javascript
@patrtorg/ex-iure.define({'text/x-abc': ['abc', 'abcd']});

@patrtorg/ex-iure.getType('abcd');            // ⇨ 'text/x-abc'
@patrtorg/ex-iure.getExtension('text/x-abc')  // ⇨ 'abc'
```

## Command Line

### Extension -> type

```bash
$ @patrtorg/ex-iure scripts/jquery.js
text/javascript
```

### Type -> extension

```bash
$ @patrtorg/ex-iure -r image/jpeg
jpeg
```
