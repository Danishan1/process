# React Native Component Replacement Cheat Sheet (HTML → RN)

## 1. Basic Structure

| HTML (Web)      | React Native Equivalent | Notes                                        |
| --------------- | ----------------------- | -------------------------------------------- |
| `<div>`         | `<View>`                | Container for layout, flexbox-based          |
| `<span>`        | `<Text>`                | No inline spans — use `<Text>` nesting       |
| `<p>`           | `<Text>`                | No automatic margins like `<p>`              |
| `<h1>` - `<h6>` | `<Text>`                | Style manually with `fontSize`, `fontWeight` |


## 2. Text & Typography

| HTML              | React Native Equivalent                            | Notes                   |
| ----------------- | -------------------------------------------------- | ----------------------- |
| `<b>`, `<strong>` | `<Text style={{fontWeight: "bold"}}>`              | No semantic meaning     |
| `<i>`, `<em>`     | `<Text style={{fontStyle: "italic"}}>`             |                         |
| `<u>`             | `<Text style={{textDecorationLine: "underline"}}>` |                         |
| `<br>`            | `{"\n"}` inside `<Text>`                           | Line break              |
| `<hr>`            | `<View style={{borderBottomWidth: 1}} />`          | Custom styling required |


## 3. Media

| HTML        | React Native Equivalent                 | Notes                           |
| ----------- | --------------------------------------- | ------------------------------- |
| `<img>`     | `<Image>`                               | Must specify `width` & `height` |
| `<video>`   | `<Video>` (via `react-native-video`)    | External lib required           |
| `<audio>`   | `<Audio>` (via packages like `expo-av`) | No native tag                   |
| `<picture>` | `<Image>` with logic                    | No direct equivalent            |


## 4. Forms & Inputs

| HTML                      | React Native Equivalent                                             | Notes                                    |
| ------------------------- | ------------------------------------------------------------------- | ---------------------------------------- |
| `<input type="text">`     | `<TextInput>`                                                       | Use `onChangeText` instead of `onChange` |
| `<textarea>`              | `<TextInput multiline />`                                           | Multiline mode                           |
| `<input type="checkbox">` | `<Switch>` or custom checkbox                                       | No native checkbox in core RN            |
| `<input type="radio">`    | Custom component (or 3rd party)                                     | No native radio input                    |
| `<select>`                | `<Picker>` (deprecated) or libraries (`react-native-picker-select`) |                                          |
| `<button>`                | `<Button>` (basic), `<TouchableOpacity>` (customizable)             | RN `<Button>` is very limited            |
| `<label>`                 | `<Text>`                                                            | No automatic label binding               |


## 5. Lists

| HTML            | React Native Equivalent                        | Notes                     |
| --------------- | ---------------------------------------------- | ------------------------- |
| `<ul>` / `<ol>` | `<FlatList>` / `<SectionList>` or map `<Text>` | No semantic list elements |
| `<li>`          | `<Text>` inside list component                 | Manual styling            |


## 6. Links & Navigation

| HTML              | React Native Equivalent                                  | Notes              |
| ----------------- | -------------------------------------------------------- | ------------------ |
| `<a>`             | `<Text onPress={...}>` or `Link` from `react-navigation` | No native `<a>`    |
| `href` navigation | `Linking.openURL("https://...")`                         | For external links |


## 7. Tables

| HTML            | React Native Equivalent                 | Notes           |
| --------------- | --------------------------------------- | --------------- |
| `<table>`       | `<View>` with flexbox grid or libraries | No native table |
| `<tr>`          | `<View style={{flexDirection: "row"}}>` |                 |
| `<td>` / `<th>` | `<Text>` in `<View>`                    | Manual styling  |


## 8. Semantic / Structural

| HTML                                                       | React Native Equivalent                       | Notes                     |
| ---------------------------------------------------------- | --------------------------------------------- | ------------------------- |
| `<header>`, `<footer>`, `<main>`, `<section>`, `<article>` | `<View>` with role only for styling/structure | No semantic meaning in RN |
| `<nav>`                                                    | Navigation container (`react-navigation`)     |                           |
| `<form>`                                                   | `<View>` wrapping `<TextInput>`s              | No submit semantics       |
| `<iframe>`                                                 | `<WebView>` (`react-native-webview`)          | Needs library             |


## 9. Others

| HTML         | React Native Equivalent                      | Notes            |
| ------------ | -------------------------------------------- | ---------------- |
| `<canvas>`   | `<Skia>` (via `react-native-skia`)           | External library |
| `<svg>`      | `<Svg>` (`react-native-svg`)                 | External library |
| `<progress>` | `<ActivityIndicator>` or custom progress bar |                  |
| `<meter>`    | Custom component                             |                  |


## 10. Example Conversion

**Web (HTML + React):**

```jsx
<div className="card">
  <img src="logo.png" alt="logo" />
  <h2>Hello World</h2>
  <button onClick={() => alert("Clicked!")}>Click Me</button>
</div>
```

**React Native:**

```jsx
<View style={styles.card}>
  <Image source={require("./logo.png")} style={{width: 100, height: 100}} />
  <Text style={styles.title}>Hello World</Text>
  <TouchableOpacity style={styles.button} onPress={() => alert("Clicked!")}>
    <Text>Click Me</Text>
  </TouchableOpacity>
</View>
```


## Key Takeaways

* RN replaces **HTML tags** with **native components**.
* Many HTML elements (form, semantic, media) don’t exist and need **libraries** or **custom components**.
* **Everything is `<View>`, `<Text>`, `<Image>` at the core**.
