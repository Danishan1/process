# React Native Events by Component (Container/Event Cheat Sheet)


## 1. **View**

General-purpose container (like `<div>`).

**Supported Events:**

* `onStartShouldSetResponder`
* `onMoveShouldSetResponder`
* `onResponderGrant` (touch start)
* `onResponderMove` (touch move)
* `onResponderRelease` (touch end)
* `onResponderTerminate`

**Use Case:**
Low-level touch/gesture handling (if not using `Pressable`/`Touchable`).


## 2. **Text**

Used for displaying text (like `<span>`/`<p>`).

**Supported Events:**

* `onPress` (tap on text)
* `onLongPress`
* `onPressIn` / `onPressOut` (via `Pressable` wrapping `Text`)
* Inherits gesture responder props (like `View`)

**Use Case:**
Clickable text links or interactive labels.

## 3. **TextInput**

Used for text entry (like `<input>`/`<textarea>`).

**Supported Events:**

* `onChangeText` (text value changes)
* `onChange` (gives event object, less common)
* `onFocus`
* `onBlur`
* `onSubmitEditing` (when “Enter/Return” pressed)
* `onKeyPress` (limited support)
* `onEndEditing`

**Use Case:**
Forms, search bars, input fields.

## 4. **TouchableOpacity / TouchableHighlight / TouchableWithoutFeedback**

Interactive containers for taps (like `<button>`).

**Supported Events:**

* `onPress`
* `onPressIn`
* `onPressOut`
* `onLongPress`

**Use Case:**
Buttons, cards, custom pressable components.

## 5. **Pressable**

Modern replacement for Touchables (more control).

**Supported Events:**

* `onPress`
* `onPressIn`
* `onPressOut`
* `onLongPress`
* `onHoverIn` / `onHoverOut` (RN Web / some platforms)
* `onFocus` / `onBlur` (for accessibility)

**Use Case:**
Preferred over `Touchable*` for granular state (hover, pressed, focused).

## 6. **ScrollView**

Scrollable container.

**Supported Events:**

* `onScroll` (with `scrollEventThrottle`)
* `onMomentumScrollBegin`
* `onMomentumScrollEnd`
* `onScrollBeginDrag`
* `onScrollEndDrag`
* `onContentSizeChange`

**Use Case:**
Lists, carousels, forms.

## 7. **FlatList / SectionList**

Optimized lists.

**Supported Events:**

* Same as `ScrollView` (`onScroll`, `onEndReached`, etc.)
* `onEndReached` (infinite scrolling)
* `onViewableItemsChanged`

**Use Case:**
Long or virtualized lists of items.

## 8. **Image**

Used for displaying images.

**Supported Events:**

* `onLoad`
* `onError`
* `onLoadStart`
* `onLoadEnd`
* `onProgress` (some platforms)

**Use Case:**
Image loading feedback, fallback images.

## 9. **Modal**

Full-screen container.

**Supported Events:**

* `onShow`
* `onDismiss` (iOS only)
* `onRequestClose` (Android back button)

**Use Case:**
Dialogs, overlays, popups.

## 10. **KeyboardAvoidingView**

Container that adjusts UI when keyboard appears.

**Supported Events:**
Doesn’t have direct events but works with `Keyboard` API:

* `Keyboard.addListener("keyboardDidShow", ...)`
* `Keyboard.addListener("keyboardDidHide", ...)`

## 11. **ActivityIndicator**

Loading spinner.

**Supported Events:**
None (purely visual).

# Example: Event Usage by Container

```jsx
<ScrollView
  onScroll={(e) => console.log("Scrolling:", e.nativeEvent.contentOffset.y)}
  scrollEventThrottle={16}
>
  <TouchableOpacity
    onPress={() => console.log("Pressed!")}
    onLongPress={() => console.log("Long Pressed!")}
  >
    <Text onPress={() => console.log("Text tapped!")}>Hello World</Text>
  </TouchableOpacity>

  <TextInput
    value={value}
    onChangeText={setValue}
    onSubmitEditing={() => console.log("Submitted")}
    onFocus={() => console.log("Focused")}
  />
</ScrollView>
```


# Key Takeaways

* **View** = lowest-level touch handling.
* **Pressable/Touchable** = for buttons/interactive UI.
* **Text** supports `onPress` like links.
* **TextInput** has rich keyboard + form events.
* **ScrollView/FlatList** provide scroll events.
* Some components (e.g., `Image`, `Modal`) have lifecycle-style events (`onLoad`, `onShow`).
