# react-native-css-transformer

Load CSS files to [react native style objects](https://facebook.github.io/react-native/docs/style.html).

## Usage

### Step 1: Install

```sh
yarn add --dev react-native-css-transformer
```

### Step 2: Configure the react native packager

Add this to your `rn-cli.config.js` (make one if you don't have one already):

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("react-native-css-transformer");
  },
  getSourceExts() {
    return ["css"];
  }
};
```

## How does it work?

Your `App.css` file might look like this:

```css
.myClass {
  color: blue;
}
.myOtherClass {
  color: red;
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
  }
};
```

You can then use that style object with an element:

```jsx
<MyElement style={styles.myClass} />
```
