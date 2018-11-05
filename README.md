# literate-json

A major downside of JSON is that it's often hard to read or annotate, especially when used as a configuration language.

One way to address this is to embed JSON data in a markdown file, using a certain format that is readable and commentable. A script takes the markdown file and converts it into one or more machine-readable JSON files.

You can also take existing JSON files and generate a markdown file from them using this format.

## Writing JSON as markdown

Define the beginning of a JSON file in your markdown with a header that is a link to a local json file, such as:

```
## [my_config.json](my_config.json)
```

Bulleted lists underneath that section are the keys and values that go within your JSON file. Similar to YAML, a colon separates key/values, and you don't need quotes. Any text outside the bulleted list are comments.

```
## [my_config.json](my_config.json)

These are the keys and values for the above JSON file:

* key1: value1
* key2: value2
```

Sub-headers below the JSON file header denote the start of nested objects.


```
## [my_config.json](my_config.json)

These are the keys and values for this JSON file:

* key1: value1
* key2: value2

### my_subobject

Everything in this section is set under the `my_subobject` key:

* subkey1: value1
* subkey2: value2
```

The above JSON is equivalent to a more compact nested list:

```
## [my_config.json](my_config.json)

* key1: value1
* key2: value2
* my_subobject:
  * subkey1: value1
  * subkey2: value2
```

You can also reduce nesting with a list by using a JSON path:

```
## [my_config.json](my_config.json)

* key1: value1
* key2: value2
* my_subobject.subkey1: value1
* my_subobject.subkey2: value2
```

If you need a key with dots in it, simply quote each part: `"parent.key"."child.key": value`.

By default, values like `true`, `false`, `null`, and `1.20` are all evaluated into their respective JSON types. If you need them to be strings, simply quote them.

Ordered lists are interpreted as arrays:

```
my_array:
  1. string1
  1. string2
  1. string3
```

Which evaluates to `{"my_array": ["string1", "string2", "string3"]}`.

Inline lists can be written as `[string1, string2, string3]`

An array of objects with multiple keys be achieved with an unordered list inside an ordered list:

```
1. ...
  * key1: val1
  * key2: val2
1. ...
  * key1: val1
  * key2: val2
```

The ellipses can be used as filler to allow for the nested unordered list.

An array of objects that each have a single key/value can be written in-line:

```
1. key: val
1. key: val
```

Which is evaluated to `[{"key": "val"}, {"key": "val"}]`

You can embed other JSON documents by using a markdown link in a value whose name has a `.md` extension:

```
# [my_json.json](my_json.json)

This is the start of the definition for the `my_json.json` file. The following is a top-level key whose contents are loaded from a separate file:

* xyz: [definitions/xyz.md](definitions/xyz.md)

```

## Examples

For a large example, see [examples/petstore-openapi.md](examples/petstore-openapi.md).

The following is an example of a readable configuration fo NPM's `package.json`:

### [package.json](package.json)

* name: watchify
* version: 3.11.0
* description: watch mode for browserify builds
* license: MIT
* homepage: https://github.com/substack/watchify
* main: index.js
* bin: bin/cmd.js
* keywords: [browserify, browserify-tool, watch, bundle, build, browser]
* repository:
  * type: git
  * url: git://github.com/substack/watchify.git

Run tests with `npm test`

* scripts:
  * test: tape test/*.js

Written by [substack](https://github.com/substack) plus [many other contributors](https://github.com/browserify/watchify/graphs/contributors)

* author:
  * name: James Halliday
  * email: mail@substack.net
  * url: http://substack.net

#### dependencies

Xkb total dependencies. Run `npm install` to install them.

* anymatch: ^1.3.0
* browserify: ^16.1.0
* chokidar: ^1.0.0
* defined: ^1.0.0
* outpipe: ^1.1.0
* through2: ^2.0.0
* xtend: ^4.0.0
* curry: 1.2.0

#### devDependencies

Required for running tests:

* brfs: ^1.0.1
* mkdirp: ~0.5.1
* split: ^1.0.0
* tape: ^4.2.2
* uglify-js: ^2.5.0
* win-spawn: ^2.0.0
