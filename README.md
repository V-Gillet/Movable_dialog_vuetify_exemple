# Movable dialogs

Based on tipsy's solution, found in the Vuetify issue : https://github.com/vuetifyjs/vuetify/issues/4058.
Since it is a basic solution with only native JS, the solution works with every framework.

This repo is an exemple of integration with Vue3, Vuetify 3 and TypeScript with some additions like a move cursor or the emptied selection while moving the dialog.

The solution is implemented in the mounted hook directly in the App.vue since the code will search for nearest dialog in the DOM in every page. To use in a different framework/version, just modify the classes in the EventTargets and query selectors.

## Project setup

```
# yarn
yarn

# npm
npm install

# pnpm
pnpm install

# bun
bun install
```

### Compiles and hot-reloads for development

```
# yarn
yarn dev

# npm
npm run dev

# pnpm
pnpm dev

# bun
bun run dev
```

### Compiles and minifies for production

```
# yarn
yarn build

# npm
npm run build

# pnpm
pnpm build

# bun
bun run build
```

### Customize configuration

See [Configuration Reference](https://vitejs.dev/config/).
