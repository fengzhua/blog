### currying

currying 的概念最早是由俄国数学家 Moses Schonfinkel 发明的，而后由著名的数学逻辑学家 Haskell Curry将其
丰富发展，currying由此得名。

`currying`又称部分求值，一个curry函数首先会接收一些参数，接收了这些参数后，该函数并不会立即求值，而是继续返回
另外一个函数，方才传入的参数在函数形成的闭包中被保存起来。待到函数被真正需要求值的时候，之前传入的所有参数都会被一次性
用于求值。

```js
var overtime = (function() {
  var args = [];

  return function() {
    if(arguments.length === 0) {
      var time = 0;
      for (var i = 0, l = args.length; i < l; i++) {
        time += args[i];
      }
      return time;
    }else {
      [].push.apply(args, arguments);
    }
  }
})();

overtime(3.5);    // 第一天
overtime(4.5);    // 第二天
overtime(2.1);    // 第三天
//...

console.log( overtime() );    // 10.1
```
以上代码是简单版本的柯里化实现方式。

### uncurrying 
在JavaScript中，当我们调用对象的某个方法时，其实不用关心该对象原本是否设计为拥有这个方法，这是动态
语言类型的特点，也是常说的鸭子类型的思想。
同理，一个对象也未必只能使用它自身的方法，那么有什么方法可以让对象去借用一个原本不属于它的方法呢？答案是
`call`和`apply`。例如`Array.prototype.slice(arguments)`。
那么有没有办法把泛化this的过程提出来？
```js
Function.prototype.uncurring = function() {
  var self = this;  //self此时是Array.prototype.push

  return function() {
    var obj = Array.prototype.shift.call(arguments);
    //obj 是{
    //  "length": 1,
    //  "0": 1
    //}
    //arguments的第一个对象被截去(也就是调用push方法的对象),剩下[2]

    return self.apply(obj, arguments);
    //相当于Array.prototype.push.apply(obj, 2);
  };
};

//测试一下
var push = Array.prototype.push.uncurring();
var obj = {
  "length": 1,
  "0" : 1
};

push(obj, 2);
console.log( obj ); //{0: 1,1: 2, length: 2 }
```
`arguments`对象本身是个伪数组，只有`length`属性，并不包含`Array`的`slice push pop`等方法，我们都需要用`Array.prototype.push.call`来实现`push`方法，但现在，直接调用push函数，既简洁又意图明了。
