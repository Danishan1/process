The **most important pattern** is not memorizing the answers. When you see a weird expression, mentally ask:

```text
1. What operator is being used?
       ↓
2. Does it perform coercion?
       ↓
3. If object → what is its primitive value?
       ↓
4. What type conversion happens?
       ↓
5. What is the final comparison/operation?
```

That process will let you solve most "trick" JavaScript questions in an interview rather than relying on memorized outputs.

### JavaScript — Complicated Comparisons

| Expression              |      Result       | Why                                                                 |
| ----------------------- | :---------------: | ------------------------------------------------------------------- |
| `1 == "1"`              |      `true`       | `==` coerces `"1"` → `1`                                            |
| `1 === "1"`             |      `false`      | Different types                                                     |
| `true == 1`             |      `true`       | `true` → `1`                                                        |
| `true === 1`            |      `false`      | Boolean vs Number                                                   |
| `false == 0`            |      `true`       | `false` → `0`                                                       |
| `false === 0`           |      `false`      | Boolean vs Number                                                   |
| `null === null`         |      `true`       | `null` only loosely equals `undefined` and strongly equal to `null` |
| `null == undefined`     |      `true`       | Special rule of `==`                                                |
| `null === undefined`    |      `false`      | Different types                                                     |
| `null == 0`             |      `false`      | `null` only loosely equals `undefined` and strongly equal to `null` |
| `undefined == 0`        |      `false`      | No coercion to `0` in this comparison                               |
| `NaN == NaN`            |      `false`      | `NaN` is unequal to itself                                          |
| `NaN === NaN`           |      `false`      | Same special rule                                                   |
| `Object.is(NaN, NaN)`   |      `true`       | `Object.is` treats NaN as same                                      |
| `0 == -0`               |      `true`       | Same numeric value                                                  |
| `0 === -0`              |      `true`       | Strict equality treats them equal                                   |
| `Object.is(0, -0)`      |      `false`      | Distinguishes signed zero                                           |
| `"5" + 2`               |      `"52"`       | `+` performs string concatenation                                   |
| `"5" - 2`               |        `3`        | `-` forces numeric conversion                                       |
| `"5" * 2`               |       `10`        | `*` forces numeric conversion                                       |
| `"5" / 2`               |       `2.5`       | `/` forces numeric conversion                                       |
| `true + 1`              |        `2`        | `true` → `1`                                                        |
| `false + 1`             |        `1`        | `false` → `0`                                                       |
| `null + 1`              |        `1`        | `null` → `0`                                                        |
| `undefined + 1`         |       `NaN`       | `undefined` → `NaN`                                                 |
| `"" == 0`               |      `true`       | `""` → `0`                                                          |
| `"0" == false`          |      `true`       | `"0"` → `0`, `false` → `0`                                          |
| `"1" == true`           |      `true`       | `"1"` → `1`, `true` → `1`                                           |
| `"10" > 5`              |      `true`       | Numeric comparison                                                  |
| `"10" > "5"`            |      `false`      | String comparison: `"1"` < `"5"`                                    |
| `"2" > "10"`            |      `true`       | Lexicographic string comparison                                     |
| `[] == false`           |      `true`       | `[]` → `""` → `0`; `false` → `0`                                    |
| `[] == 0`               |      `true`       | `[]` → `""` → `0`                                                   |
| `[] == ""`              |      `true`       | `[]` → `""`                                                         |
| `[] === false`          |      `false`      | Object vs Boolean                                                   |
| `[] === []`             |      `false`      | Different object references                                         |
| `{} === {}`             |      `false`      | Different object references                                         |
| `const a={}; a===a`     |      `true`       | Same reference                                                      |
| `[1] == 1`              |      `true`       | `[1]` → `"1"` → `1`                                                 |
| `[1,2] == "1,2"`        |      `true`       | Array → `"1,2"`                                                     |
| `[1,2] == 1`            |      `false`      | `"1,2"` → `NaN`                                                     |
| `![]`                   |      `false`      | Objects are truthy                                                  |
| `!{}`                   |      `false`      | Objects are truthy                                                  |
| `!![]`                  |      `true`       | Array is truthy                                                     |
| `!!{}`                  |      `true`       | Object is truthy                                                    |
| `[] + []`               |       `""`        | Both arrays → empty strings                                         |
| `[1] + [2]`             |      `"12"`       | Arrays → strings → concatenation                                    |
| `[1,2] + [3,4]`         |    `"1,23,4"`     | Arrays converted to strings                                         |
| `{} + []`               | Context-dependent | Parsing/statement-vs-expression issue                               |
| `0.1 + 0.2 === 0.3`     |      `false`      | Floating-point precision                                            |
| `Infinity > 999999`     |      `true`       | Infinity is greater                                                 |
| `Infinity === Infinity` |      `true`       | Same numeric value                                                  |
| `typeof NaN`            |    `"number"`     | NaN is a Number value                                               |
| `typeof null`           |    `"object"`     | Historical JS behavior                                              |
| `typeof []`             |    `"object"`     | Arrays are objects                                                  |
| `typeof function(){}`   |   `"function"`    | Special callable-object result                                      |
| `[] instanceof Array`   |      `true`       | Array prototype chain                                               |
| `{ } instanceof Array`  |      `false`      | Not an Array                                                        |
| `Array.isArray([])`     |      `true`       | Direct array check                                                  |

### The ones I'd memorize first

These are particularly common interview traps:

```js
[] == false       // true
[] == 0           // true
[] == ""          // true

[1] == 1          // true
[1, 2] == "1,2"   // true

"5" + 2           // "52"
"5" - 2           // 3

true + 1          // 2
null + 1          // 1
undefined + 1     // NaN

null == undefined // true
null == 0         // false

NaN === NaN       // false

[] === []         // false
{} === {}         // false

0 === -0          // true
Object.is(0,-0)   // false

Object.is(NaN,NaN) // true
```
