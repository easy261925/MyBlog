---
title: 关于Array的ES5部分方法的实现
date: 2018-01-13 13:30:47
tags:
---
# 关于Array的ES5部分方法的实现

> forEach

```js
if (typeof Array.prototype.forEach !== &quot;function&quot;) {
    Array.prototype.myForEach = function (fn) {
        for (var i = 0; i &lt; this.length; i++) {
            // console.log(Object.prototype);
            // console.log(Object.prototype.hasOwnProperty.call(this, i))
            if (typeof fn === &quot;function&quot;) {
                if (Object.prototype.hasOwnProperty.call(this, i)) {
                    fn.call(this,this[i], i, this);
                }
            }
            else {
                throw new Error(fn + &quot; is not a function&quot;);
            }
        }
    }
 }
var array = [1, 2, 3, 4, 5, 6];
array.myForEach(function (value, index, array) {
    console.log(value);
    console.log(index);
    console.log(array);
})
```
> map

```js
if (typeof Array.prototype.map !== &quot;function&quot;) {
Array.prototype.myMap = function (fn) {
        var temp = [];
        for (var i = 0; i &lt; this.length; i++) {
            if (typeof fn === &quot;function&quot;) {
                if (Object.prototype.hasOwnProperty.call(this, i)) {
                    temp.push(fn.call(this, this[i], i, this));
                }
            } else {
                throw new Error(fn + &quot; is not a function&quot;);
            }
        }
        return temp;
    }
}
var arr1 = [1, 2, 3, 4, 5, 6];
var arr2 = arr1.myMap(function (value) {
    return value * 2;
})
console.log(arr2);
```

> filter

```js
if(typeof Array.prototype.filter !== &quot;function&quot;){
	Array.prototype.myFilter = function(fn){
        var temp = [];
        for(var i=0;i&lt;this.length;i++){
            if(typeof fn === &quot;function&quot;){
                if(Object.prototype.hasOwnProperty.call(this,i)){
                    if(fn.call(this,this[i])){
                        temp.push(this[i]);
                    }
                }
            }
        }
        return temp;
    }
 }
var arr1 = [1,2,3,4,45,89,123,45,8,10];
var arr2 = arr1.myFilter(function(value){
    return value &lt; 5;
})
console.log(arr2);
```

