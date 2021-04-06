**Do NOT run `yarn install` manually - everything is ready to go!**

The purpose of this repository is to understand implications of switching from explicit exact Flow types (`{||}`) to implicit exact Flow types (`{}`) for our published NPM libraries.

# Test 1 (the default `exact_by_default=false`)

Change `node_modules/@adeira/test/index.js` to:

```js
// @flow

export type A = {| x: string |} // exact
export type B = { x: string, ... } // inexact
export type C = { x: string } // inexact
```

And `.flowconfig` to:

```text
[options]
exact_by_default=false
```

Result:

```text
Error ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈ index.js:5:14

Cannot assign object literal to a because property y is missing in A [1] but exists in object literal [2].
[prop-missing]

        2│
        3│ import type {A, B, C} from '@adeira/test'
        4│
 [1][2] 5│ const a: A = {x:"", y:""}
        6│ const b: B = {x:"", y:""}
        7│ const c: C = {x:"", y:""}
        8│

```

# Test 2 (`exact_by_default=true`)

Change `node_modules/@adeira/test/index.js` to:

```js
// @flow

export type A = {| x: string |} // exact
export type B = { x: string, ... } // inexact
export type C = { x: string } // exact
```

And `.flowconfig` to:

```text
[options]
exact_by_default=true
```

Result:

```text
Error ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈ index.js:5:14

Cannot assign object literal to a because property y is missing in A [1] but exists in object literal [2].
[prop-missing]

        2│
        3│ import type {A, B, C} from '@adeira/test'
        4│
 [1][2] 5│ const a: A = {x:"", y:""}
        6│ const b: B = {x:"", y:""}
        7│ const c: C = {x:"", y:""}
        8│


Error ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈ index.js:7:14

Cannot assign object literal to c because property y is missing in C [1] but exists in object literal [2].
[prop-missing]

         4│
         5│ const a: A = {x:"", y:""}
         6│ const b: B = {x:"", y:""}
 [1][2]  7│ const c: C = {x:"", y:""}
         8│

```

# Test 3 (converted NPM with the default `exact_by_default=false`)

Change `node_modules/@adeira/test/index.js` to:

```js
// @flow

export type A = { x: string } // inexact (CHANGED)
export type B = { x: string, ... } // inexact
export type C = { x: string } // inexact
```

And `.flowconfig` to:

```text
[options]
exact_by_default=false
```

Result:

```text
No errors!
```

# Test 4 (converted NPM with `exact_by_default=true`)

Change `node_modules/@adeira/test/index.js` to:

```js
// @flow

export type A = { x: string } // exact (CHANGED)
export type B = { x: string, ... } // inexact
export type C = { x: string } // exact
```

And `.flowconfig` to:

```text
[options]
exact_by_default=true
```

Result:

```text
Error ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈ index.js:5:14

Cannot assign object literal to a because property y is missing in A [1] but exists in object literal [2].
[prop-missing]

        2│
        3│ import type {A, B, C} from '@adeira/test'
        4│
 [1][2] 5│ const a: A = {x:"", y:""}
        6│ const b: B = {x:"", y:""}
        7│ const c: C = {x:"", y:""}
        8│


Error ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈ index.js:7:14

Cannot assign object literal to c because property y is missing in C [1] but exists in object literal [2].
[prop-missing]

         4│
         5│ const a: A = {x:"", y:""}
         6│ const b: B = {x:"", y:""}
 [1][2]  7│ const c: C = {x:"", y:""}
         8│

```

# Conclusion

## Users with `exact_by_default=false` (legacy default)

Change from explicit exact objects (`{||}`) to implicit exact objects (`{}`) can have unintended effects. Such users should convert their codebase as soon as possible:

- first to explicit exact objects (`{||}`) and explicit inexact objects (`{...}`)
- second from explicit exact objects (`{||}`) to implicit exact objects (`{}`) while keeping explicit _inexact_ objects

## Users with `exact_by_default=true` (the old new)

There is no change and everything should be good to go. ✅
