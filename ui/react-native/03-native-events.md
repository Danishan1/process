Perfect — let’s prepare the **Events Cheat Sheet** just like we did with HTML tags.
This will map **DOM events in React (web)** → **React Native event system**.

---

# React Native Events Cheat Sheet (Web → RN)

React Native does not have a DOM, so event names and props differ.
Here’s a mapping:

---

## 1. Mouse & Click Events

| React (Web)     | React Native Equivalent  | Notes                                                 |
| --------------- | ------------------------ | ----------------------------------------------------- |
| `onClick`       | `onPress`                | Used with `Button`, `TouchableOpacity`, `Pressable`   |
| `onDoubleClick` | `onLongPress` (closest)  | Long-press instead of double-click                    |
| `onMouseEnter`  | `onHoverIn` (Pressable)  | Limited support; often emulate with state             |
| `onMouseLeave`  | `onHoverOut` (Pressable) | Works only with certain RN components or web platform |

---

## 2. Keyboard Events

| React (Web)             | React Native Equivalent                           | Notes                                   |
| ----------------------- | ------------------------------------------------- | --------------------------------------- |
| `onKeyDown`             | `onKeyPress` / `onKeyDown` (limited on TextInput) | Keyboard events only inside text inputs |
| `onKeyUp`               | `onKeyPress` (not identical)                      | No global keyboard handling (mobile)    |
| `onInput` (text change) | `onChangeText`                                    | Fired with the new value directly       |

---

## 3. Form Events

| React (Web)        | React Native Equivalent    | Notes                                         |
| ------------------ | -------------------------- | --------------------------------------------- |
| `onChange` (input) | `onChangeText` (TextInput) | Directly gives new string                     |
| `onSubmit` (form)  | `onSubmitEditing`          | Fires when pressing Enter/Return in TextInput |
| `onFocus`          | `onFocus`                  | Works for TextInput                           |
| `onBlur`           | `onBlur`                   | Works for TextInput                           |

---

## 4. Touch & Gesture Events

| React (Web)         | React Native Equivalent                             | Notes                          |
| ------------------- | --------------------------------------------------- | ------------------------------ |
| `onTouchStart`      | `onPressIn`                                         | Fires when touch begins        |
| `onTouchEnd`        | `onPressOut`                                        | Fires when touch ends          |
| `onTouchMove`       | `onResponderMove`                                   | Lower-level event for gestures |
| `onDrag` / `onDrop` | Use `PanResponder` / `react-native-gesture-handler` | No built-in drag/drop like web |

---

## 5. Focus Events

| React (Web)               | React Native Equivalent | Notes                  |
| ------------------------- | ----------------------- | ---------------------- |
| `onFocus`                 | `onFocus`               | Supported on TextInput |
| `onBlur`                  | `onBlur`                | Supported on TextInput |
| `onFocusIn`, `onFocusOut` | Not available           | Must be simulated      |

---

## 6. Clipboard & Input

| React (Web) | React Native Equivalent | Notes                    |
| ----------- | ----------------------- | ------------------------ |
| `onCopy`    | No direct equivalent    | Use `Clipboard` API      |
| `onPaste`   | No direct equivalent    | Also via `Clipboard` API |
| `onCut`     | No direct equivalent    |                          |

---

## 7. Form Submission Example

**Web:**

```jsx
<form onSubmit={(e) => { e.preventDefault(); alert("Submitted") }}>
  <input type="text" onChange={(e) => setValue(e.target.value)} />
</form>
```

**React Native:**

```jsx
<TextInput
  value={value}
  onChangeText={setValue}
  onSubmitEditing={() => alert("Submitted")}
/>
```

---

## 8. Touch Button Example

**Web:**

```jsx
<button onClick={() => alert("Clicked")}>Click Me</button>
```

**React Native:**

```jsx
<TouchableOpacity onPress={() => alert("Pressed!")}>
  <Text>Click Me</Text>
</TouchableOpacity>
```

---

## 9. Key Differences to Remember

* **`onClick` → `onPress`**
* **Text input events differ** (`onChange` vs `onChangeText`)
* **No hover/keyboard by default** (since it’s mobile-first)
* **Complex gestures** need `react-native-gesture-handler` or `PanResponder`

---

Would you like me to **combine the HTML tags + Events sheets** into a single **“React to React Native migration handbook”** so your devs get one compact reference?
