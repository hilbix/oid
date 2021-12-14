# OID reference

Some [OIDs](https://en.wikipedia.org/wiki/Object_identifier) I reference.

For example, my ([Valentin Hilbig](https://github.com/hilbix/))
own OID is [1.3.6.1.4.1.8505.1.568](1/3/6/1/4/1/8505/1/568/)


## Layout

The layout is simple:

- An OID is made out of nodes which are written hierarchically with separating dots.
- Nodes are made out of a number which might have a name assigned.
- We stick to the number, the name is kept as metadata.
- `NODE`s are written in decimal without leading zeros.  Zero is written as `0`.  There are no negative numbers.
- OIDs are stored by a path with the nodes creating a directory `NODE`
- Metadata of a node is put into a `NODE.json` file.  This is optional.
- Each `NODE.json` file has a given structure (see JSON below).
- Information is scanned top-down, that is if several `NODE.json` files apply,
  the ones in the higher level directories are read first,
  and the ones in the lower level directories can overwrite or augment the data.
- Each `NODE.json` cannot contain information of OIDs which follow the `NODE`.
  - If so this is a bug and the information is invalid.
  - It is unspecified what happens with JSON files with invalid information.
  - Either such JSON is ignored completely or only the valid information is used.
  - This should not happen.

If you want to give some information about some OID, place a `README.md` file
(in Markdown format) in the so created directory.

Additional files may be present, too.

> Please abstain to create an empty `.gitignore` at the level.
> Instead use a `.json` file at a level above with empty `META` information (hence being `{}`)


## JSON

The JSON format is simple as well:

- Encoding always is `UTF-8` without BOM.
  - An existing BOM should be treated gracefully but nevertheless is a bug.
- The extension `.json` must be lowercase
- It contains a JSON object with a single entry at the top level.
- The single entry's name is the OID up to here.
  - For example the file `1/3/6/1/4/1/8505.json` looks like `{"1.3.6.1.4.1.8505":META}`
    with `META` beeing the Metadata Object in JSON format.
  - Seing `{"1.3.6.1.4.1.8505.1.568":META}` here would be wrong.
  - This is a requirement, so the node's path and the OID in the name of the JSON object need to match.
- `META` being just `{}` is ok, but it must not be `[]` nor `""` nor some other JSON type like `null`.

The Metadata Object's names are either:

- Starting with a number which refers to some NODE or NODEs.
  - Which then is additional `META` data.
  - These must form valid OID node parts (must not end on a dot etc.)
- Or with an ASCII letter (case sensitive) followed by AlphaNum which is used for metadata fields
  - Length is 1 character to 255 characters
- Or contains some underscores `_`, which are reserved for Metadata Metainformation.
  - Names which start with `_` are reserved (for things like CouchDB)
- Or start with a `/` or `%` character.  These are ignored.
  - Recommendation is that those strings are encoded according to JavaScript's `encodeURIComponent()`
  - These could show up here to keep some information around until the underlying problems are resolved.
- All other names (even that we support UniCode) are wrong
- This can be applied recursively.

Note that there is no requirement to recurse on each level.  Something like `{"1.3.6":{"1.4.1.8505.1":META}}` and
`{"1.3.6":{"1":{"4":{"1":{"8505":{"1":META}}}}}}` contain the same information at `1.3.6.json`.

All OID node parts must not share a common nonempty prefix on the same level of the Metadata:

- Hence the `"1.4.1.8505.1"` means, there is
  - no `"1"` nor some `"1.4"`, nor some `"1.4.1"`, nor some `"1.4.1.8505"`
  - nor some `"1.4.1.8505.2"`, nor `"1.4.1.8506"` or similar.
  - But there can be some `"2"` or `"0"`, as those do not share a common prefix with `"1.4.1.8505.1"`.

## Common names

> Use `/x` for now to present your idea

T.B.D.

## Reserved names

> For now, Temporary (instable) reserved names end on `_`

T.B.D.

## FAQ

License?

- All information in this repo is [Public Domain](https://creativecommons.org/publicdomain/zero/1.0/)
- As OIDs only make sense when the basic information is in public use, there cannot be copyrighted anyway

More?

- If you like to add your OID information, clone, add your OID information,
  send a PR stating that the information is not covered by any Copyright
