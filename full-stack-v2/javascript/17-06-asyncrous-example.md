
```js
console.log('begins');
 
setTimeout(() => {
  console.log('setTimeout 1');
  Promise.resolve().then(() => {
    console.log('promise 1');
  });
}, 0);
 
new Promise(function (resolve, reject) {
  console.log('promise 2');
  setTimeout(function () {
    console.log('setTimeout 2');
    resolve('resolve 1');
  }, 0);
}).then((res) => {
  console.log('dot then 1');
  setTimeout(() => {
    console.log(res); 
  }, 0);
});
```

```js
console.log("start"); // 1

Promise.resolve().then(() => {
    console.log("promise 1"); // 5
    Promise.resolve().then(() => {
        console.log("promise 2"); // 6
    });
});

console.log("end"); // 2

process.nextTick(() => {
    console.log("nextTick 1"); // 3
    process.nextTick(() => {
        console.log("nextTick 2"); // 4
    });
});
```

```js
console.log("A"); // 1

process.nextTick(() => {
    console.log("B"); // 3

    Promise.resolve().then(() => {
        console.log("C"); // 5
    });
});

Promise.resolve().then(() => {
    console.log("D");  // 4

    process.nextTick(() => {
        console.log("E"); // 6
    });
});

console.log("F"); // 2

```