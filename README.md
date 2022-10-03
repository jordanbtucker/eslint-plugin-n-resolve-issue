This is a minimal reproducible example of a bug in `eslint-plugin-n`.

The `resolvePaths` shared setting does not work the same for `import` statements
as it does for `require` statements.

For `import` statements, the `resolvePaths` setting works the same as the
`paths` option in Node.js `require.resolve`. In this example, `resolvePaths` is
set to `['./lib']` so when `src/esm.mjs` imports `./index.js`, it does so as if
`esm.mjs` was in `lib`, and `./index.js` correctly resolves to `./lib/index.js`.

For `require` statements, the `resolvePaths` setting does not work this way. In
this example, `src/cjs.js` tries to require `./index.js` as if `cjs.js` was in
`lib`, but eslint throws an error.

To see the error, run:

```
npm run lint
```

---

Note that running `src/esm.mjs` or `src/cjs.js` will throw an error because
`index.js` is in a different directory, but that's not the point of this
example. A possible use case where this setup may exist is in a TypeScript
project where a `.ts` file imports a `.js` file relative to the location where
its `.js` file be transpiled.
