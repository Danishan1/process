# React vs React Native: Side-by-Side Guide for React Developers

This guide helps **React (web)** developers transition into **React Native** quickly by comparing concepts, APIs, and syntax.


## 1. Project Structure & Entry Point

| React (Web)                                               | React Native                                                                  |
| --------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Entry file is usually `index.js` with `ReactDOM.render()` | Entry file is `index.js` (or `App.js`) with `AppRegistry.registerComponent()` |
| Renders into a DOM node (`<div id="root">`)               | Renders into a native host app (no DOM)                                       |
| Bundled with Webpack/Vite/CRA                             | Bundled with Metro                                                            |


## 2. Components

| React (Web)                                            | React Native                                                                |
| ------------------------------------------------------ | --------------------------------------------------------------------------- |
| Uses **HTML elements** (`<div>`, `<span>`, `<button>`) | Uses **React Native components** (`<View>`, `<Text>`, `<TouchableOpacity>`) |
| DOM elements are styled with CSS                       | Native components are styled with `StyleSheet` or CSS-in-JS                 |
| Browser handles rendering                              | Native views (`UIView` on iOS, `View` on Android)                           |

Example:

```jsx
// React (Web)
<div className="container">
  <p>Hello World</p>
</div>

// React Native
<View style={styles.container}>
  <Text>Hello World</Text>
</View>
```

## 3. Styling

| React (Web)                                         | React Native                                                                             |
| --------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| CSS, SCSS, CSS Modules, styled-components, Tailwind | No CSS. Use `StyleSheet`, `styled-components/native`, or NativeWind (Tailwind for RN)    |
| Supports selectors, cascade, media queries          | No selectors/cascade. Styles are JS objects. Media queries via `Dimensions` or libraries |
| Units: `px`, `%`, `em`, `rem`, `vh`, `vw`           | Unitless numbers (dp). `%` partially supported. No `em`, `rem`, `vh`, `vw`               |
| Animations: CSS transitions, keyframes              | Animations: `Animated`, `LayoutAnimation`, `react-native-reanimated`                     |


## 4. Events

| React (Web)                           | React Native                                               |
| ------------------------------------- | ---------------------------------------------------------- |
| `onClick`, `onChange`, `onMouseEnter` | `onPress`, `onChangeText`, `onPressIn` / `onPressOut`      |
| Browser events bubble through DOM     | RN events are handled at component level (no DOM bubbling) |
| Example: `<button onClick={...}>`     | Example: `<TouchableOpacity onPress={...}>`                |


## 5. Routing

| React (Web)                       | React Native                                            |
| --------------------------------- | ------------------------------------------------------- |
| React Router (`react-router-dom`) | React Navigation (`@react-navigation/native`)           |
| URL-based navigation              | Stack/tab/drawer navigators (no browser URL by default) |



## 6. Platform APIs

| React (Web)                                     | React Native                                                                                            |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Access browser APIs (localStorage, fetch, etc.) | Access native APIs via RN libraries (`AsyncStorage`, `react-native-camera`, `react-native-geolocation`) |
| DOM APIs (`document.querySelector`, etc.)       | No DOM. Use refs + native modules instead                                                               |
| CSS media queries for responsive design         | Use `Dimensions`, `useWindowDimensions`, or responsive libraries                                        |


## 7. Build & Deployment

| React (Web)                   | React Native                                            |
| ----------------------------- | ------------------------------------------------------- |
| Runs in browser               | Compiled to iOS/Android apps                            |
| Build with Vite, CRA, Webpack | Build with Metro bundler, Xcode (iOS), Gradle (Android) |
| Deployment via web server     | Deployment via App Store / Google Play                  |


## 8. Example: Button

```jsx
// React (Web)
<button
  className="btn"
  onClick={() => alert("Clicked!")}
>
  Click Me
</button>

// React Native
<TouchableOpacity
  style={styles.button}
  onPress={() => alert("Pressed!")}
>
  <Text>Click Me</Text>
</TouchableOpacity>
```

## 9. Example: Text Input

```jsx
// React (Web)
<input
  type="text"
  value={value}
  onChange={e => setValue(e.target.value)}
/>

// React Native
<TextInput
  value={value}
  onChangeText={setValue}
  style={styles.input}
/>
```


## 10. Key Mindset Shifts

1. **No DOM, no HTML** → Think in terms of *native components* (`View`, `Text`, `Image`).
2. **No CSS cascade** → Styles are scoped JS objects, no global stylesheets.
3. **Platform differences** → iOS and Android can render differently (fonts, shadows, navigation).
4. **Animations & gestures** → Handled with dedicated RN APIs or libraries.
5. **Navigation ≠ Routing** → Stack-based navigation instead of URLs.


## 11. Recommended Libraries for Web Devs Transitioning

* **Navigation**: `react-navigation`
* **Styling**: `styled-components/native` or `nativewind` (Tailwind)
* **State Management**: Redux Toolkit, Zustand, Jotai (same as web)
* **Forms**: `react-hook-form`, `formik` (RN-compatible)
* **Animations**: `react-native-reanimated`, `react-native-gesture-handler`


## 12. Takeaways

* If you know React, **70% carries over**.
* The main changes: **no DOM, no CSS, platform APIs instead of browser APIs**.
* Think: *React for native mobile* rather than *React for the web with CSS*.
