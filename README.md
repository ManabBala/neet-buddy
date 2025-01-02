<div align="center">

# NEET Prep Assistant
### A Chrome Extension to Help NEET Aspirants Stay Focused

</div>

## Features

- 🎯 YouTube Focus Mode
  - Hides distracting elements (comments, recommendations, etc.)
  - Only shows educational content related to NEET subjects
  - Custom filters for NEET-relevant channels
  - Shortcuts to open youtube in focus mode
  - Other various features

- ⏰ Study Timer & Reminders
  - Pomodoro timer optimized for NEET preparation
  - Break reminders
  - Daily study tracking

- 📚 Quick Access Tools
  - NCERT chapter quick links
  - Formula sheets
  - Previous year questions
  - Subject-wise bookmarks

- 🚫 Distraction Blocker
  - Block non-educational websites during study hours
  - Customizable blocking schedule
  - Emergency override with password

---

## Getting started

1. When you're using Windows run this:
   - `git config --global core.eol lf`
   - `git config --global core.autocrlf input`

   **This will set the EOL (End of line) character to be the same as on Linux/macOS. Without this, our bash script won't work, and you will have conflicts with developers on Linux/macOS.**
2. Clone this repository.
3. Check your node version is >= than in `.nvmrc` file, recommend to use [nvm](https://github.com/nvm-sh/nvm?tab=readme-ov-file#intro)
4. Install pnpm globally: `npm install -g pnpm` (check your node version >= 22.12.0))
5. Run `pnpm install`

Then, depending on the target browser:

### For Chrome: <a name="getting-started-chrome"></a>

1. Run:
    - Dev: `pnpm dev` (on Windows, you should run as administrator; see [issue#456](https://github.com/Jonghakseo/chrome-extension-boilerplate-react-vite/issues/456))
    - Prod: `pnpm build`
2. Open in browser - `chrome://extensions`
3. Check - <kbd>Developer mode</kbd>
4. Click - <kbd>Load unpacked</kbd> in the upper left corner
5. Select the `dist` directory from the boilerplate project

### For Firefox: <a name="getting-started-firefox"></a>

1. Run:
    - Dev: `pnpm dev:firefox`
    - Prod: `pnpm build:firefox`
2. Open in browser - `about:debugging#/runtime/this-firefox`
3. Click - <kbd>Load Temporary Add-on...</kbd> in the upper right corner
4. Select the `./dist/manifest.json` file from the boilerplate project

> [!NOTE]
> In Firefox, you load add-ons in temporary mode. That means they'll disappear after each browser close. You have to load the add-on on every browser launch.

## Install dependency for turborepo: <a name="install-dependency"></a>

### For root: <a name="install-dependency-for-root"></a>

1. Run `pnpm i <package> -w`

### For module: <a name="install-dependency-for-module"></a>

1. Run `pnpm i <package> -F <module name>`

`package` - Name of the package you want to install e.g. `nodemon` \
`module-name` - You can find it inside each `package.json` under the key `name`, e.g. `@extension/content-script`, you can use only `content-script` without `@extension/` prefix

## Environment variables

To add an environment variable:

1. Copy `.example.env` to `.env` (in the same directory)
2. Add a new record inside `.env`, prefixed with `VITE_`, e.g. `VITE_MY_API_KEY=...`
3. Edit `./vite-env.d.ts` and in the `ImportMetaEnv` interface, add your variable with the appropriate type, e.g.

   `readonly VITE_MY_API_KEY: string;`
4. Then you can read the variable via `import.meta.env.VITE_MY_API_KEY` (learn more at [Env Variables and Modes](https://vite.dev/guide/env-and-mode))

#### If you want to set it for each package independently:

1. Create `.env` inside that package
2. Open related `vite.config.mts` and add `envDir: '.'` at the end of this config
3. Rest steps like above

#### Remember you can't use global and local at the same time for the same package(It will be overwritten)

## Project structure <a name="structure"></a>

### Chrome extension <a name="structure-chrome-extension"></a>

The extension lives in the `chrome-extension` directory and includes the following files:

- [`manifest.js`](chrome-extension/manifest.js) - script that outputs the `manifest.json`
- [`src/background`](chrome-extension/src/background) - [background script](https://developer.chrome.com/docs/extensions/mv3/background_pages/) 
  (`background.service_worker` in manifest.json)
- [`public`](chrome-extension/public/) - icons referenced in the manifest; content CSS for user's page injection

### Pages <a name="structure-pages"></a>

Code that is transpiled to be part of the extension lives in the [pages](pages/) directory.

- [`content`](pages/content/) - [content scripts](https://developer.chrome.com/docs/extensions/develop/concepts/content-scripts)
  (`content_scripts` in manifest.json)
- [`content-ui`](pages/content-ui) - React UI rendered in the current page (you can see it at the very bottom when you get started)
  (`content_scripts` in manifest.json)
- [`content-runtime`](pages/content-runtime/src/) - [injected content scripts](https://developer.chrome.com/docs/extensions/develop/concepts/content-scripts#functionality);
  this can be injected from `popup` like standard `content`
- [`options`](pages/options/) - [options page](https://developer.chrome.com/docs/extensions/develop/ui/options-page)
  (`options_page` in manifest.json)
- [`popup`](pages/popup/) - [popup](https://developer.chrome.com/docs/extensions/reference/api/action#popup) shown when clicking the extension in the toolbar
  (`action.default_popup` in manifest.json)
- [`side-panel`](pages/side-panel/) - [sidepanel (Chrome 114+)](https://developer.chrome.com/docs/extensions/reference/api/sidePanel)
  (`side_panel.default_path` in manifest.json)

### Packages <a name="structure-packages"></a>

Some shared packages:

- `dev-utils` - utilities for Chrome extension development (manifest-parser, logger)
- `i18n` - custom internationalization package; provides i18n function with type safety and other validation
- `hmr` - custom HMR plugin for Vite, injection script for reload/refresh, HMR dev-server
- `shared` - shared code for the entire project (types, constants, custom hooks, components etc.)
- `storage` - helpers for easier integration with [storage](https://developer.chrome.com/docs/extensions/reference/api/storage), e.g. local/session storages
- `tailwind-config` - shared Tailwind config for entire project
- `tsconfig` - shared tsconfig for the entire project
- `ui` - function to merge your Tailwind config with the global one; you can save components here
- `vite-config` - shared Vite config for the entire project
- `zipper` - run `pnpm zip` to pack the `dist` folder into `extension-YYYYMMDD-HHmmss.zip` inside the newly created `dist-zip`
- `e2e` - run `pnpm e2e` for end-to-end tests of your zipped extension on different browsers

## Reference

- [Chrome Extensions](https://developer.chrome.com/docs/extensions)
- [Vite Plugin](https://vitejs.dev/guide/api-plugin.html)
- [Rollup](https://rollupjs.org/guide/en/)
- [Turborepo](https://turbo.build/repo/docs)
- [Rollup-plugin-chrome-extension](https://www.extend-chrome.dev/rollup-plugin)