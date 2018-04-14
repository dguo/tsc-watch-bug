# tsc watch bug
This repo is a minimal reproduction of what seems to be a bug with the
TypeScript compiler's watch mode. For some reason, this particular set of
dependencies in `package.json` causes `tsc` to watch files that aren't targeted
for compilation.

## Reproduction steps
1. I used Node v8.6.0, Yarn v1.5.1, and TypeScript v2.8.1.
2. Clone this repo, and run `yarn install`.
3. Run `yarn tsc -w` or `yarn tsc -w src/foo.ts`. The bug should occur either way.
4. Make a change in `package.json`, `bar.js`, or
   `node_modules/ioredis/index.js`, and save.
5. tsc should update with: `File change detected. Starting incremental
   compilation...`

## Comments
1. `tsc` seems to process the type definitions in `node_modules`, even though
   `foo.ts` doesn't import any package. This behavior was surprising to me.
2. I originally ran into this issue in a project repo. I attempted to trim down
   the dependencies to a single offending one, but it seems like something
   about this combination is not good. Removing any more dependencies fixed the
   issue during my testing.
3. I tried using `typescript@next`, but the issue still occurs.
