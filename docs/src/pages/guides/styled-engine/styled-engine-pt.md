# `@mui/styled-engine`

<p class="description">Configuring your preferred styling library.</p>

The default style library used for generating CSS styles for MUI components is [emotion](https://github.com/emotion-js/emotion). All of the MUI components rely on the `styled()` API to inject CSS into the page. This API is supported by multiple popular styling libraries, which makes it possible to switch between them in MUI.

## How to switch to styled-components

If you already have [styled-components](https://github.com/styled-components/styled-components) installed, it's possible to use it exclusively. There are currently two packages available to choose from:

- `@mui/styled-engine` - a thin wrapper around [emotion's `styled()`](https://emotion.sh/docs/styled) API, with the addition of few other required utilities, such as the `<GlobalStyles />` component, the `css` and `keyframe` helpers, etc. This is the default.
- `@mui/styled-engine-sc` - a similar wrapper around `styled-components`.

These two packages implement the same interface, which makes it possible to replace one with the other. By default, `@mui/material` has `@mui/styled-engine` as a dependency, but you can configure your bundler to replace it with `@mui/styled-engine-sc`.

### yarn

If you are using yarn, you can configure it using a package resolution:

**package.json**

<!-- #default-branch-switch -->

```diff
 {
   "dependencies": {
-    "@mui/styled-engine": "latest"
+    "@mui/styled-engine": "npm:@mui/styled-engine-sc@latest"
   },
+  "resolutions": {
+    "@mui/styled-engine": "npm:@mui/styled-engine-sc@latest"
+  },
 }
```

### npm

As package resolutions are not available in npm at this moment, you need to update you bundler's config to add this alias. Here is an example of how you can do it, if you use `webpack`:

**webpack.alias.js**

```diff
 module.exports = {
   //...
+  resolve: {
+    alias: {
+      '@mui/styled-engine': '@mui/styled-engine-sc'
+    },
+  },
 };
```

If you are using TypeScript, you will need to also update the TSConfig.

**tsconfig.json**

```diff
 {
   "compilerOptions": {
+    "paths": {
+      "@mui/styled-engine": ["./node_modules/@mui/styled-engine-sc"]
+    }
   },
 }
```

### Next.js

**next.config.js**

```diff
+const withTM = require('next-transpile-modules')([
+  '@mui/material',
+  '@mui/system',
+  '@mui/icons-material', // If @mui/icons-material is being used
+]);

+module.exports = withTM({
 webpack: (config) => {
   config.resolve.alias = {
     ...config.resolve.alias,
+    '@mui/styled-engine': '@mui/styled-engine-sc',
    };
    return config;
  }
+});
```

### Ready-to-use examples

If you are using create-react-app, there is a ready-to-use template in the example projects.

You can use these `styled-component` examples as a reference:

<!-- #default-branch-switch -->

- [create-react-app](https://github.com/mui-org/material-ui/tree/master/examples/create-react-app-with-styled-components)
- [create-react-app with TypeScript](https://github.com/mui-org/material-ui/tree/master/examples/create-react-app-with-styled-components-typescript)
- [Next.js](https://github.com/mui-org/material-ui/tree/master/examples/nextjs-with-styled-components-typescript)
- [and many others](https://github.com/mui-org/material-ui/tree/master/examples)

> **Note**: `@emotion/react`, `@emotion/styled`, and `styled-components` are optional peer dependencies of `@mui/material`, so you need to install them yourself. See the [Installation guide](/getting-started/installation/) for more info.

> **Note:** This package-swap approach is identical to the replacement of React with [Preact](https://github.com/preactjs/preact). The Preact team has documented a large number of installation configurations. If you are stuck with MUI + styled-components, don't hesitate to check out how they solve the problem, as you can likely transfer the solution.
