# Jest Babel Bug

This is a super weird Jest/Babel bug.

## Steps to reproduce

- Clone this repository
- Run `yarn` to install dependencies
- Run `yarn jest` and note that the single test passes without any issues

Now it gets weird

- Go into `/packages/foobar` and rename `package.json.test` to `package.json`
- Open `/.babelrc` and edit the file in any way (eg. add some white space, a new line, a traling comma). Just something to reset the Jest cache. This step is very important. If you don't touch this file, the issue will not be reproducible.
- Run `yarn jest` again

**BUG**: Now the exact same test fails because Jest doesn't process the code through babel anymore. The failure is:

```
● Test suite failed to run

    Jest encountered an unexpected token

    This usually means that you are trying to import a file which Jest cannot parse, e.g. it's not plain JavaScript.

    By default, if Jest sees a Babel config, it will use that to transform your files, ignoring "node_modules".

    Here's what you can do:
     • To have some of your "node_modules" files transformed, you can specify a custom "transformIgnorePatterns" in your config.
     • If you need a custom transformation specify a "transform" option in your config.
     • If you simply want to mock your non-JS modules (e.g. binary assets) you can stub them out with the "moduleNameMapper" config option.

    You'll find more details and examples of these config options in the docs:
    https://facebook.github.io/jest/docs/en/configuration.html

    Details:

    C:\Users\globe\Documents\Cumul8\Git\jest-babel-bug\packages\aurora-core\main.test.js:1
    ({"Object.<anonymous>":function(module,exports,require,__dirname,__filename,global,jest){import { A } from './main';
                                                                                                    ^

    SyntaxError: Unexpected token {

      at ScriptTransformer._transformAndBuildScript (node_modules/jest-runtime/build/script_transformer.js:403:17)
```

## TLDR

So it seems like, if you have a `package.json` file (even if it's mostly empty) inside your project, Jest will fail to process content through Babel. This is serious issue for anyone working with Lerna projects or other monorepo projects.
