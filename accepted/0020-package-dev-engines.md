# Package `devEngines`

## Summary

Add optional `devEngines` field to package manifest schema
for definition of system-level constraint criteria satisfactory
for package's development. Based on field's contents, warn of
missing/unsupported tools in developer's active Node.js toolchain.

## Motivation

As [stated by the documentation](https://docs.npmjs.com/files/package.json#engines), the optional `engines` field "is advisory only and will only produce warnings when your package is installed as a dependency."

```console
npm WARN notsup Unsupported engine for package@1.0.0: wanted: {"node":"x.x.x","npm":"x.x.x"} (current: {"node":"x.x.x","npm":"x.x.x"})
npm WARN notsup Not compatible with your version of node/npm: package@1.0.0
```
*The actual advisory warning printed to the console.*

- This advisory warning displays regardless of whether a package is installed as `dependency` or `devDependency`.
- This optional `engines` field exists for definition of engines suitable for a package's **consumption** intended to encompass packages defined in its `dependencies` field (except those containing a `prepare` lifecycle script).
- No equivalent field exists for definition of engine criteria for a package's **development** that is intended to encompass its usage (and packages defined in its `devDependencies` field's usage) of more modern platform features.
- A project that both publishes a package and develops that package within the
same project repository is capable of developing said package to be compatible with Node.js toolchain versions older than the one used for development.

How then, should one specify those engines suitable for development, but not consumption of this package?

## Detailed Explanation

Projects authored in the latest iteration of ECMAScript, that perform a build step transpiling down to ES3, would publish a package containing modules suitable for **running** on versions of Node.js as low as 0.10. As this package would list a `devDependency` of `@babel/core`, which has an `engines` constraint of `>=6.9.0` for `node`, its **development** would not be supported by Node.js 0.10. The current `engines` field, therefore, cannot serve a dual-purpose of specifying those engines compatible with both its `dependencies` _and_ `devDependencies` as the resulting SemVer range would be inaccurate.

> If the package being installed contains a `prepare` script, its `dependencies` and `devDependencies` will be installed, and the prepare script will be run, before the package is packaged and installed.  
> — [npm-install Description](https://docs.npmjs.com/cli/install#description)

Published packages containing a `prepare` lifecycle script install both `dependencies`
and `devDependencies`, which would be the only case where it would be appropriate for a package to specify an `engines` field intended to encompass both `dependencies` and `devDependencies` as they would both be installed and run during the execution of this script.

## Rationale and Alternatives

<!-- Discuss 2-3 different alternative solutions that were considered. This is required, even if it seems like a stretch. Then explain why this is the best choice out of available ones. -->

1. Implement this field as a `preinstall` lifecycle script without npm's blessing.
2. Use inaccurate SemVer range that satisfies both `dependencies` and `devDependencies`.

<!-- TODO: Then explain why this is the best choice out of available ones. -->

## Implementation

{{Give a high-level overview of implementation requirements and concerns. Be specific about areas of code that need to change, and what their potential effects are. Discuss which repositories and sub-components will be affected, and what its overall code effect might be.}}

{{THIS SECTION IS REQUIRED FOR RATIFICATION -- you can skip it if you don't know the technical details when first submitting the proposal, but it must be there before it's accepted}}




## Prior Art

{{This section is optional if there are no actual prior examples in other tools}}

{{Discuss existing examples of this change in other tools, and how they've addressed various concerns discussed above, and what the effect of those decisions has been}}

## Unresolved Questions and Bikeshedding

{{Write about any arbitrary decisions that need to be made (syntax, colors, formatting, minor UX decisions), and any questions for the proposal that have not been answered.}}

{{THIS SECTION SHOULD BE REMOVED BEFORE RATIFICATION}}
