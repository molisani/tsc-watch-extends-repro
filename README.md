# tsc-watch-extends-repro

This repo provides a reproduction of a bug with `tsc` where it fails to pick up changes to a file referenced by `extends` from a `tsconfig.json`.

## Steps

0. Install `typescript` with `npm install`

1. Start watching project

Ensure `root.json` has `compilerOptions.strict` set to `true`, then `npm run watch` to start `tsc -w` for this `tsconfig.json`.

2. Save `tsconfig.json` w/o changes

While watching the project, saving the `tsconfig.json` file should trigger `tsc` to detect a file change. It should still find one error:

> ```
> File change detected. Starting incremental compilation...
> index.ts:1:7 - error TS2322: Type 'null' is not assignable to type 'number'.
>
> 1 const x: number = null;
>         ~
>
> Found 1 error. Watching for file changes.
> ```

3. Change `root.json`, observe bug

Change `root.json` such that `compilerOptions.strict` is `false`. This also disables `strictNullChecks` and as such should not report the above as an error. **Changing this file will not trigger a file change for the project watcher.**

If you then save `tsconfig.json` (still without changes) it will now pick up the latest version of `root.json` and will no longer report any errors.

> ```
> File change detected. Starting incremental compilation...
>
> Found 0 errors. Watching for file changes.
> ```
