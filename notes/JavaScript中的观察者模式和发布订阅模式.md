## JavaScript中的观察者模式和发布订阅模式

[案例展示](https://codesandbox.io/s/javascriptguanchazhehefabudingyuemoshi-btk95?file=/index.html)

> 观察者模式与发布订阅模式都是定义了一个一对多的依赖关系，当有关状态发生变更时则执行相应的更新。发布订阅模式比观察者模式要更加灵活多变。
>
> 观察者模式和发布订阅模式最大的区别:发布订阅模式有个事件调度中心。

### 观察者模式（Observer）

该模式的思想来源于**DOM2的事件处理**。

在DOM0的阶段，只能给一个元素绑定一个有效的方法，再绑定的话后边会覆盖前面的。

```javascript
Div.onclick = function(){}
```

在DOM2中，是基于事件池机制的，可以绑定多个方法，另外DOM2还可以绑定一些特殊事件的绑定方法，如：DOMContentLoaded、transitionend

```javascript
Div.addEventListener("click", function(){
    //  fn();
    //  fn1();
    //	可以添加多个事件
})
```

> 观察者模式创建一个被观察者对象(broadcast)和多个观察者对象(Observer)，当broadcast状态发生变化则通知一系列的Observer进行更新。



```javascript
let Broadcast = (function(){
    class Sub {
        constructor() {
            // 创建事件队列，用来存储后来需要执行的方法
            this.queue = [];
        }
        // 向事件队列添加监听方法
        add(fn) {
            if(!this.queue.includes(fn)){
                this.queue.push(fn)
            }
        }
        // Broadcast进行事件广播
        notify(...args){
            let queue = this.queue;
            for(let i=0;i<queue.length;i++){
                let item = queue[i]
                if (typeof item !== "function") {
                    queue.splice(i, 1);
                    i--;
                    continue;
                }
                item.call(this, ...args);
            }
        }
        // 从事件队列中移除事件方法
        remove(fn) {
            let queue = this.queue;
            for (let i = 0; i < queue.length; i++) {
                let item = queue[i];
                if (item == fn) {
                    // queue.splice(i, 1);
                    queue[i] = null;
                    break;
                }
            }
        }
    }
    return function(){
        return new Sub()
    }
})()
let queue = Broadcast();
let fn1 = function() {
    console.log("fn1");
};
let fn2 = function() {
    console.log("fn2");
    // queue.remove(fn1);
};
let fn3 = function() {
    console.log("fn3");
};
let fn4 = function() {
    console.log("fn4");
};
queue.add(fn1);
queue.add(fn2);
queue.add(fn3);
queue.add(fn4);

queue.notify();  // fn1  fn2  fn3  fn4
```

### 发布订阅模式

> 发布订阅模式指的: 订阅者（Subscriber）基于一个主题通过自定义事件订阅主题，发布者对象（Publisher）通过发布主题事件的方式通知各个订阅者（Subscriber）订阅过该主题的 Subscriber 对象。

使用一个村里的广播事件演示

```javascript
var VillageRadio = {
  clienlist: {} /*缓存列表*/,
  /**
   * 增加订阅者
   * @key {String} 类型
   * @fn {Function} 回掉函数
   * */
  on: function(key, fn) {
    if (!this.clienlist[key]) {
      this.clienlist[key] = [fn];
    } else {
      this.clienlist[key].push(fn);
    }
  },
  /**
   * 发布消息
   * */
  emit: function() {
    var key = [].shift.call(arguments), //取出emit第一个的参数，即事件类型
      fns = this.clienlist[key]; //取出该类型的对应的消息集合
    if (!fns || fns.length === 0) {
      return false;
    }
    fns.forEach(fn => {
      fn.apply(this, arguments);
    });
  },
  /**
   * 删除订阅
   * @key {String} 类型
   * @fn {Function} 回掉函数
   * */
  remove: function(key, fn) {
    var fns = this.clienlist[key]; //取出该类型的对应的消息集合
    if (!fns) {
      //如果对应的key没有订阅直接返回
      return false;
    }
    if (!fn) {
      //如果没有传入具体的回掉，则表示需要取消所有订阅
      fns && (fns.length = 0);
    } else {
      fns.forEach((item, idx) => {
        if (item === fn) {
          fns.splice(idx, 1);
        }
      });
    }
  }
};

/*村民张三订阅事件*/
function VillagerZhangsan(name, time) {
  console.log("张三订阅放电影：电影名-<<" + name + ">>，时间是：" + time);
}
/*村民李四订阅事件*/
function VillagerLisi(name, time) {
  console.log("李四订阅放电影：电影名-<<" + name + ">>，时间是：" + time);
}
// VillageRadio.on("Villager", data => {
//   console.log("村民订阅事件");
// });
VillageRadio.on("Villager", VillagerZhangsan);
VillageRadio.on("Villager", VillagerLisi);

VillageRadio.on("要停电", VillagerZhangsan);
VillageRadio.on("要停电", VillagerLisi);
document.querySelector(".subscribe1").addEventListener("click", function() {
  VillageRadio.emit("Villager", "我和我的祖国", "19:30");
  VillageRadio.remove("要停电", VillagerLisi); //注意这里是按照地址引用的。如果传入匿名函数则删除不了
  VillageRadio.emit("要停电", "后天");
});
```

村民张三和李四可以分别订阅村广播的不同事件

发布订阅模式比观察者模式多了一个事件类型的判断

[](./image/watchAndPublish.png)



