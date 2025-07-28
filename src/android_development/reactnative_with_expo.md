# reactnative with expo

## create the project

create a new project with expo, you can replace `myNewAndroidProject` with any name :

```
npx create-expo-app@latest myNewAndroidProject
```

Navigate to the project with `cd myNewAndroidProject` .

## install Tailwindcss on react-native

### install the  package

Install nativewind and it's dependencies :

```
npm install nativewind tailwindcss@^3.4.17 react-native-reanimated@3.16.2 react-native-safe-area-context
```

### setup Tailwindcss

Run :

```
npx tailwindcss init
```

And add the path of your components :

```
/** @type {import('tailwindcss').Config} */
module.exports = {
  // NOTE: Update this to include the paths to all of your component files.
  content: ["./app/**/*.{js,jsx,ts,tsx}"],
  presets: [require("nativewind/preset")],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

create a css file and add the directives `global.css` :

```tailwind
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Add the babel preset

`babel.config.js` :

```
module.exports = function (api) {
  api.cache(true);
  return {
    presets: [
      ["babel-preset-expo", { jsxImportSource: "nativewind" }],
      "nativewind/babel",
    ],
  };
};
```

### modify your metro.config.js

create a file `metro.config.js` :

> [!WARNING] Attention !
> you MUST replace the `./global.css` file with the actual file path which is in my case `./app/global.css` file . if you don't , your styles won't be applied.

```
const { getDefaultConfig } = require("expo/metro-config");
const { withNativeWind } = require('nativewind/metro');
 
const config = getDefaultConfig(__dirname)
 
module.exports = withNativeWind(config, { input: './global.css' })
```

### Import the css file

import the file in the layout , you cannot import it with `"@/"` import , you must do it manually :

```
import "./global.css";
```

### typescript support

create a file named `nativewind-env.d.ts` in order for typescript support and put this as content :

```
/// <reference types="nativewind/types" />
```







