# react-native-css-transformer [![NPM version](http://img.shields.io/npm/v/react-native-css-transformer.svg)](https://www.npmjs.org/package/react-native-css-transformer) [![Downloads per month](https://img.shields.io/npm/dm/react-native-css-transformer.svg)](http://npmcharts.com/compare/react-native-css-transformer?periodLength=30) [![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://egghead.io/courses/how-to-contribute-to-an-open-source-project-on-github)

Use CSS to style your React Native apps.

Behind the scenes the `.css` files are transformed to [react native style objects](https://facebook.github.io/react-native/docs/style.html) (look at the examples).

> This transformer can be used together with [React Native CSS modules](https://github.com/kristerkari/react-native-css-modules).

## How does it work?

Your `App.css` file might look like this:

```css
.myClass {
  color: blue;
}
.myOtherClass {
  color: red;
}
.my-dashed-class {
  color: green;
}
```

When you import your stylesheet:

```js
import styles from "./App.css";
```

Your imported styles will look like this:

```js
var styles = {
  myClass: {
    color: "blue"
  },
  myOtherClass: {
    color: "red"
  },
  "my-dashed-class": {
    color: "green"
  }
};
```

You can then use that style object with an element:

**Plain React Native:**

```jsx
<MyElement style={styles.myClass} />

<MyElement style={styles["my-dashed-class"]} />
```

**[React Native CSS modules](https://github.com/kristerkari/react-native-css-modules) using [className](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style) property:**

```jsx
<MyElement className={styles.myClass} />

<MyElement className={styles["my-dashed-class"]} />
```

**[React Native CSS modules](https://github.com/kristerkari/react-native-css-modules) using [styleName](https://github.com/kristerkari/babel-plugin-react-native-stylename-to-style) property:**

```jsx
<MyElement styleName="myClass my-dashed-class" />
```

## Installation and configuration

### Step 1: Install

```sh
npm install --save-dev react-native-css-transformer
```

or

```sh
yarn add --dev react-native-css-transformer
```

### Step 2: Configure the react native packager

#### For Expo SDK v41.0.0 or newer

Merge the contents from your project's `metro.config.js` file with this config (create the file if it does not exist already).

`metro.config.js`:

```js
const { getDefaultConfig } = require("expo/metro-config");

module.exports = (() => {
  const config = getDefaultConfig(__dirname);

  const { transformer, resolver } = config;

  config.transformer = {
    ...transformer,
    babelTransformerPath: require.resolve("react-native-css-transformer")
  };
  config.resolver = {
    ...resolver,
    sourceExts: [...sourceExts, "css"]
  };

  return config;
})();
```

---

#### For React Native v0.72.1 or newer

Merge the contents from your project's `metro.config.js` file with this config (create the file if it does not exist already).

`metro.config.js`:

```js
const { getDefaultConfig, mergeConfig } = require("@react-native/metro-config");

const defaultConfig = getDefaultConfig(__dirname);
const { assetExts, sourceExts } = defaultConfig.resolver;

/**
 * Metro configuration
 * https://reactnative.dev/docs/metro
 *
 * @type {import('metro-config').MetroConfig}
 */
const config = {
  transformer: {
    babelTransformerPath: require.resolve("react-native-css-transformer")
  },
  resolver: {
    sourceExts: [...sourceExts, "css"]
  }
};

module.exports = mergeConfig(defaultConfig, config);
```

## CSS Custom Properties (CSS variables)

```css
:root {
  --text-color: blue;
}

.blue {
  color: var(--text-color);
}
```

CSS variables are not supported by default, but you can add support for them by using [PostCSS](https://postcss.org/) and [postcss-css-variables](https://github.com/MadLittleMods/postcss-css-variables#readme) plugin.

Start by installing dependencies:

```sh
yarn add postcss postcss-css-variables react-native-postcss-transformer --dev
```

Remove CSS transformer from the project:

```sh
yarn remove react-native-css-transformer
```

Add `postcss-css-variables` to your PostCSS configuration with [one of the supported config formats](https://github.com/michael-ciniawsky/postcss-load-config), e.g. `package.json`, `.postcssrc`, `postcss.config.js`, etc.

After that replace the transformer name in your Metro config file (`metro.config.js`):

```diff
-require.resolve("react-native-css-transformer")
+require.resolve("react-native-postcss-transformer")
```

## Dependencies

This library has the following Node.js modules as dependencies:

- [css-to-react-native-transform](https://github.com/kristerkari/css-to-react-native-transform)
