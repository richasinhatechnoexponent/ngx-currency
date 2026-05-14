# ngx-currency

[![npm version](https://badge.fury.io/js/ngx-currency.png)](http://badge.fury.io/js/ngx-currency)
[![GitHub issues](https://img.shields.io/github/issues/nbfontana/ngx-currency.png)](https://github.com/nbfontana/ngx-currency/issues)
[![GitHub stars](https://img.shields.io/github/stars/nbfontana/ngx-currency.png)](https://github.com/nbfontana/ngx-currency/stargazers)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.png)](https://raw.githubusercontent.com/nbfontana/ngx-currency/master/LICENSE)

## Demo

https://nbfontana.github.io/ngx-currency/

## Table of contents

- [Getting Started](#getting-started)
- [Documentation](https://nbfontana.github.io/ngx-currency/docs/)
- [Development](#development)
- [Publishing to npm](#publishing-to-npm)
- [License](#license)

## Getting Started

### Installing and Importing

Install the package by command:

```sh
npm install ngx-currency --save
```

Import the directive

```ts
import { NgxCurrencyDirective } from "ngx-currency";

@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  imports: [NgxCurrencyDirective],
})
export class AppComponent {}
```

### Using

```html
<input type="text" inputmode="decimal" currencyMask formControlName="value" />
```

- `ngModel` An attribute of type number. If is displayed `'$ 25.63'`, the attribute will be `'25.63'`.

### Options

You can set options...

```html
<!-- example for pt-BR money -->
<input [currencyMask]="{ prefix: 'R$ ', thousands: '.', decimal: ',' }" formControlName="value" />
```

Available options:

- `align` - Text alignment in input. (default: `right`)
- `allowNegative` - If `true` can input negative values. (default: `true`)
- `decimal` - Separator of decimals (default: `'.'`)
- `precision` - Number of decimal places (default: `2`)
- `prefix` - Money prefix (default: `'$ '`)
- `suffix` - Money suffix (default: `''`)
- `thousands` - Separator of thousands (default: `','`)
- `nullable` - when true, the value of the clean field will be `null`, when false the value will be `0`
- `min` - The minimum value (default: `undefined`)
- `max` - The maximum value (default: `undefined`)
- `inputMode` - Determines how to handle numbers as the user types them (default: `Financial`)

Input Modes:

- `Financial` - Numbers start at the highest precision decimal. Typing a number shifts numbers left.
  The decimal character is ignored. Most cash registers work this way. For example:
  - Typing `'12'` results in `'0.12'`
  - Typing `'1234'` results in `'12.34'`
  - Typing `'1.234'` results in `'12.34'`
- `Natural` - Numbers start to the left of the decimal. Typing a number to the left of the decimal shifts
  numbers left; typing to the right of the decimal replaces the next number. Most text inputs
  and spreadsheets work this way. For example:
  - Typing `'1234'` results in `'1234'`
  - Typing `'1.234'` results in `'1.23'`
  - Typing `'12.34'` results in `'12.34'`
  - Typing `'123.4'` results in `'123.40'`

You can also set options globally...

```ts
import { provideEnvironmentNgxCurrency, NgxCurrencyInputMode } from 'ngx-currency';

bootstrapApplication(AppComponent, {
  providers: [
    ...
    provideEnvironmentNgxCurrency({
      align: "right",
      allowNegative: true,
      allowZero: true,
      decimal: ",",
      precision: 2,
      prefix: "R$ ",
      suffix: "",
      thousands: ".",
      nullable: true,
      min: null,
      max: null,
      inputMode: NgxCurrencyInputMode.Financial,
    }),
    ...
  ],
}).catch((err) => console.error(err));
```

## Development

### Prepare your environment

- Install [Node.js](http://nodejs.org/) and NPM
- Install local dev dependencies: `npm install` while current directory is this repo

### Development server

To start a local development server, run:

```bash
npm start
```

### Building

To build the library run:

```bash
npm run build:lib
```

### Testing

To execute unit tests with the [Karma](https://karma-runner.github.io) test runner, use the following command:

```bash
npm test
```

When running in the Chrome browser, you can set code breakpoints to debug tests using these instructions:

- From the main Karma browser page, click the `Debug` button to open the debug window
- Press `ctrl + shift + i` to open Chrome developer tools
- Press `ctrl + p` to search for a file to debug
- Enter a file name like `input.handler.ts` and click the file
- Within the file, click on a row number to set a breakpoint
- Refresh the browser window to re-run tests and stop on the breakpoint

## Publishing to npm

These steps apply to publishing the **library** built into `dist/ngx-currency/` (see `projects/ngx-currency/package.json` for the package `name`, for example `@sumond25/ngx-currency`). Do **not** publish the private demo app at the repo root.

### Before you publish

1. Create an account on [npmjs.com](https://www.npmjs.com/) if you do not have one.
2. For a **scoped** package (`@scope/name`), the scope must match your npm **username** or an **organization** you control. Adjust `name` in `projects/ngx-currency/package.json` if needed, then rebuild.
3. The first time you publish a public scoped package, you may need:  
   `npm access public --scope=@your-scope`
4. Create a **granular access token** (or classic token, if you still use one) with permission to **publish** this package. See [npm access tokens](https://docs.npmjs.com/about-access-tokens). If publish is blocked with `403` and a message about **2FA**, use a token that allows publishing under your account’s 2FA rules, or pass a one-time code with `--otp` (below).

### Build the package

From the repository root:

```bash
npm install
npm run build:lib
```

This refreshes `dist/ngx-currency/` (including `package.json`, bundles, and typings).

### `.npmrc` layout (project vs user)

- The **repo** `.npmrc` should keep only **non-secret** settings (for example `legacy-peer-deps`). **Do not** commit `//registry.npmjs.org/:_authToken=…` to the repository.
- Put your **token in your user-level** `.npmrc` (for example on Windows: `C:\Users\<you>\.npmrc`; on macOS/Linux: `~/.npmrc`). npm merges [multiple npmrc files](https://docs.npmjs.com/cli/v10/using-npm/npmrc#files); you **do not** need to copy `.npmrc` into `dist/ngx-currency/`. Publishing from that folder still picks up the user config when npm walks up the directory tree.
- After `npm run build:lib`, `dist/ngx-currency/.npmignore` includes `.npmrc` so a stray token file in `dist/` is unlikely to be packed into the tarball.

### Authentication with a token (recommended)

1. On npmjs.com, create a **granular** token with **publish** access to your package (or scope).
2. In your **user** `~/.npmrc`, add a single line (no quotes):

   ```ini
   //registry.npmjs.org/:_authToken=npm_your_token_here
   ```

   Or run once (writes to user config):

   ```bash
   npm config set //registry.npmjs.org/:_authToken=npm_your_token_here
   ```

3. Confirm npm sees you:

   ```bash
   npm whoami
   ```

You **do not** need `npm login` for this flow; that command starts a separate browser-based sign-in and does not replace a token in `~/.npmrc`.

### Check the tarball (optional)

```bash
cd dist/ngx-currency
npm pack --dry-run
```

### Publish

```bash
cd dist/ngx-currency
npm publish --access public
```

If npm requires a 2FA code for this publish:

```bash
npm publish --access public --otp=123456
```

Replace `123456` with the current code from your authenticator app.

### CI (GitHub Actions, etc.)

Store the token as a secret (for example `NPM_TOKEN`), then before `npm publish`:

```bash
npm config set //registry.npmjs.org/:_authToken="${NPM_TOKEN}"
```

Or append that line to `~/.npmrc` in the job only. Never commit the token or echo it into files tracked by git.

### Version bumps

npm rejects a publish if that **exact version** already exists. Bump `version` in `projects/ngx-currency/package.json`, run `npm run build:lib` again, then publish from `dist/ngx-currency`.

## License

MIT @ Neri Bez Fontana
