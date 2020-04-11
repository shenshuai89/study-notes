## JavaScript数据类型基础和堆栈内存底层原理

### 数据类型基础









### 堆栈内存底层原理







```javascript
let a = {
    n:10
}
let b = a;
b.m = b = {
    n: 20
}
console.log(a)  // {n:10,m:{n:20}}
console.log(b)  // {n:20}
```



```javascript
let x = [12,23]
function fn(y){
  y[0] = 111;
  y = [111];
  y[1] = 200;
  console.log(y)
}
fn(x) // [111,200]
console.log(x)  // [111,23]
```

