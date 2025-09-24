# React Native StyleSheet: A Guide for CSS Experts

This document is designed for developers with strong **CSS knowledge** who are transitioning to **React Native**. While React Native styling borrows concepts from CSS, it is **not CSS** — it is a JavaScript API with its own rules and limitations.


## 1. Core Principles

* React Native does **not use CSS files** (`.css`, `.scss`, `.module.css` won’t work).
* Styles are written as **JavaScript objects**, usually via `StyleSheet.create()`.
* There is **no cascade**, **no selectors**, and **no inheritance** (except for a few text properties).

Example:

```jsx
import { StyleSheet, View, Text } from "react-native";

export default function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Hello React Native</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,                // fills available space
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "#fff",
  },
  text: {
    color: "blue",
    fontSize: 18,
  },
});
```


## 2. What Works (Carryover from CSS)

### Layout & Box Model

* Flexbox (`display: flex` is implicit in RN)
* `flex`, `flexDirection`, `justifyContent`, `alignItems`
* `margin`, `padding`, `borderWidth`, `borderColor`, `borderRadius`

### Positioning

* `position: "absolute" | "relative"`
* `top`, `bottom`, `left`, `right`, `zIndex`

### Colors

* Supports `hex`, `rgb()`, `rgba()`, named colors (`"red"`, `"blue"`)

### Typography

* `fontSize`, `fontWeight`, `fontStyle`
* `fontFamily` (must use installed fonts)
* `lineHeight`, `letterSpacing`
* `textAlign`, `textDecorationLine`, `textTransform`


## 3. What Does Not Exist in StyleSheet

| CSS Concept                                    | React Native Equivalent                                                                      |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Selectors** (`.class`, `#id`, `div > span`)  | None — apply styles directly to components                                                   |
| **Pseudo-classes** (`:hover`, `:focus`)        | Handled in JS logic (e.g., state for pressed button)                                         |
| **Pseudo-elements** (`::before`, `::after`)    | Create extra `<View>`/`<Text>` components                                                    |
| **Media Queries** (`@media`)                   | Use `Dimensions`, `useWindowDimensions`, or libraries like `react-native-responsive-screen`  |
| **CSS Variables (`--var`)**                    | Use JS constants or theme context                                                            |
| **Keyframe Animations (`@keyframes`)**         | Use `Animated`, `LayoutAnimation`, or `react-native-reanimated`                              |
| **Grid Layout**                                | Not available, flexbox only (unless you use a custom lib)                                    |
| **Functions (`calc()`, `clamp()`, etc.)**      | Compute in JS before applying style                                                          |
| **Units (`px`, `em`, `rem`, `%`, `vh`, `vw`)** | Unitless numbers (interpreted as dp). `%` works in flex layouts. No `em`, `rem`, `vh`, `vw`. |


## 4. Things That Work Differently

### Units

* React Native uses **unitless numbers** = density-independent pixels.

  ```js
  fontSize: 16   // not "16px"
  ```

### Shadows

* iOS:

  ```js
  shadowColor: "#000",
  shadowOffset: { width: 0, height: 2 },
  shadowOpacity: 0.25,
  shadowRadius: 3.84,
  ```
* Android:

  ```js
  elevation: 5
  ```

### Z-Index

* Works only within the same parent.
* No browser-style stacking contexts.

### Inheritance

* Only some **Text** properties inherit (e.g., `color`, `fontSize` if nested in `<Text>`).
* Layout styles (`margin`, `padding`) never inherit.


## 5. How to Handle Missing Features

### Media Queries

```js
import { useWindowDimensions } from "react-native";
const { width } = useWindowDimensions();

const styles = StyleSheet.create({
  text: {
    fontSize: width > 400 ? 20 : 14,
  },
});
```

### CSS Variables

```js
const COLORS = {
  primary: "#3498db",
  secondary: "#2ecc71",
};

const styles = StyleSheet.create({
  button: {
    backgroundColor: COLORS.primary,
  },
});
```

### Hover / Focus States

Use component state + event handlers:

```jsx
<TouchableOpacity
  onPressIn={() => setPressed(true)}
  onPressOut={() => setPressed(false)}
  style={[styles.button, pressed && styles.buttonPressed]}
/>
```

### Animations

```js
import { Animated } from "react-native";
const fadeAnim = useRef(new Animated.Value(0)).current;

useEffect(() => {
  Animated.timing(fadeAnim, {
    toValue: 1,
    duration: 1000,
    useNativeDriver: true,
  }).start();
}, []);
```


## 6. When You Miss CSS

If you want a **CSS-like developer experience**, consider:

* **Styled Components** (`styled-components/native`)
* **Tailwind for RN** via [NativeWind](https://www.nativewind.dev/)
* **react-native-extended-stylesheet (EStyleSheet)** — supports variables & media queries


## 7. Key Takeaways

* Think of RN’s StyleSheet as a **strict subset of CSS**.
* Everything is **local, scoped, and JS-driven**.
* For complex styling (themes, responsive, animations), rely on **JavaScript logic** or **specialized libraries**.
