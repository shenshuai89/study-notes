## JavaScript对象的拷贝

### 浅拷贝Object.assign

> Object.assign 方法只会拷贝源对象自身的并且可枚举的属性到目标对象。

``` js
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy);  // { a: 1 }
```



### 实现一个深拷贝deepClone函数

```javascript
function deepClone(source) {
  // 如果是日期对象或者正则表达式，则直接返回原对象
  if (source instanceof Date || source instanceof RegExp) return source;
  let target = Array.isArray(source) ? [] : {};
  for (let key in source) {
    // 判断属性是自身的，不是原型链的
    if (source.hasOwnProperty(key) && source !== null) {
      if (typeof source[key] === "object") {
        target[key] = deepClone(source[key]);
      } else {
        target[key] = source[key];
      }
    }
  }
  return target;
}
```



> 注意：JSON.stringify()和JSON.parse()结合使用实现深拷贝不靠谱，过程中会将属性为undefined和函数的类型给舍弃掉。

```js
let obj1 = {
    x:1,
    arr:[1,2,3],
    y:undefined,
    fn:function(){console.log("log")}
}
let obj2= JSON.parse(JSON.stringify(obj1))
console.log(obj1,obj2)
```

