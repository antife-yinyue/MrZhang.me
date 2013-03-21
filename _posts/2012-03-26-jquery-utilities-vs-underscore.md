---
layout: post
date: 2012-03-26 15:50:00
title: jQuery Utilities vs Underscore
tags:
- jQuery
- Underscore
---

抛开其他因素，只简单比较下各方法本身。能力有限，可能会有偏颇，望不吝赐教～

jQuery版本是1.7.2，Underscore版本是1.3.1。

## $.each() vs _.each()

两者都是用于遍历，使用方法也基本一样，不同之处是index和value的顺序不一样：<br />jQuery：`$.each(list, function(index, value) {})`<br />Underscore：`_.each(list, function(value, index) {})`

后者优先使用了ECMAScript 5的原生`forEach`方法，性能上也就优于前者。

## $.extend() vs _.extend()

功能类似，但前者支持深度合并，看例子：

```js
var obj1 = {
  name: '{{ site.author }}',
  site: {
    blog: 'http://MrZhang.me',
    engine: 'GitHub + Octopress'
  }
}
var obj2 = {
  site: {
    weibo: 'http://weibo.com/{{ site.sina_weibo_id }}'
  }
}

console.log( _.extend({}, obj1, obj2) )
//=> { name: '{{ site.author }}', site: { weibo: 'http://weibo.com/{{ site.sina_weibo_id }}' } }

console.log( $.extend({}, obj1, obj2) )
//=> { name: '{{ site.author }}', site: { weibo: 'http://weibo.com/{{ site.sina_weibo_id }}' } }

// 深度合并
console.log( $.extend(true, {}, obj1, obj2) )
//=> { name: '{{ site.author }}', site: { blog: 'http://MrZhang.me', engine: 'GitHub + Octopress', weibo: 'http://weibo.com/{{ site.sina_weibo_id }}' } }
```

<!-- more -->

## $.grep() vs _.filter() + _.reject()

这次是一对二，`$.grep()`第三个参数传入`true`时，就达到了`_.reject()`的效果。`_.filter()`优先使用了ECMAScript 5的原生`filter`方法。

```js
var arr = [1, 2, 3, 4, 5, 6]
var fn = function(num) { return num % 2 == 0 }

console.log( _.filter(arr, fn) )
console.log( $.grep(arr, fn) )
//=> [2, 4, 6]

console.log( _.reject(arr, fn) )
console.log( $.grep(arr, fn, true) )
//=> [1, 3, 5]
```

## $.inArray() vs _.indexOf()

两者都优先使用了ECMAScript 5的原生`indexOf`方法，他们的语法分别是：<br />jQuery：`$.inArray(value, array, [fromIndex])`<br />Underscore：`_.indexOf(array, value, [isSorted])`

可见，除了value和array的位置不一样外，第三个参数也是不一样的。前者的`fromIndex`的是用于指定遍历的起始索引，后者的`isSorted`的是用于开启[二分查找法](http://baike.baidu.com/view/1195050.htm)功能。Underscore还有个相关的`_.lastIndexOf()`。

## $.isArray() vs _.isArray()

这次两者的功能、代码都是一样的，那就没什么好说的了～

## $.isFunction() vs _.isFunction()

同上～Next～

## $.isNumeric() vs _.isNumber()

先看例子：

```js
console.log( $.isNumeric('-10') )
//=> true
console.log( _.isNumber('-10') )
//=> false

console.log( $.isNumeric('0xFF') )
//=> true
console.log( _.isNumber('0xFF') )
//=> false

console.log( $.isNumeric('8e5') )
//=> true
console.log( _.isNumber('8e5') )
//=> false

console.log( $.isNumeric(NaN) )
//=> false
console.log( _.isNumber(NaN) )
//=> true

console.log( $.isNumeric(Infinity) )
//=> false
console.log( _.isNumber(Infinity) )
//=> true

console.log( $.isNumeric(+false) )
//=> true
console.log( _.isNumber(+false) )
//=> true

console.log( $.isNumeric(null) )
//=> false
console.log( _.isNumber(null) )
//=> false

console.log( $.isNumeric(undefined) )
//=> false
console.log( _.isNumber(undefined) )
//=> false
```

前者会先对传入的参数执行`parseFloat()`，所以前三组返回的都是`true`，然后执行`!isNaN()`，所以第四组就返回`false`，同时它还使用`isFinite()`，第五组就也返回`false`了。

后者则是纯粹的类型判断：`Object.prototype.toString.call()`。

## $.makeArray() vs _.toArray()

两者的区别在于，当传入的参数是Hash时，前者是直接将Hash作为数组的一个元素，而后者则是将Hash的values组成数组。

```js
var obj = {
  name: '{{ site.author }}',
  blog: 'http://MrZhang.me'
}

console.log( $.makeArray(obj) )
//=> [{ name: '{{ site.author }}', blog: 'http://MrZhang.me' }]
console.log( _.toArray(obj) )
//=> ['{{ site.author }}', 'http://MrZhang.me']
```

## $.map() vs _.map()

两者功能一致，也都返回数组，但后者优先使用ECMAScript 5的原生`map`方法，性能优于前者。

## $.merge() vs _.union()

两者都是用于合并数组，不同之处在于：前者只接受两个参数，把第二个数组合并到第一个之后，将改变第一个数组，不去重，所以新第一个数组的元素的个数是先前两个数组的总和；后者接受任意个参数，去重，并返回新的数组，不改变先前的数组。

```js
console.log( $.merge([1, 2, 3], [101, 2, 1, 10]) )
//=> [1, 2, 3, 101, 2, 1, 10]

console.log( _.union([1, 2, 3], [101, 2, 1, 10], [2, 1]) )
//=> [1, 2, 3, 101, 10]
```
