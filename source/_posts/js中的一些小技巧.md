---
title: js中的一些小技巧
date: 2019-05-24 17:43:05
tags:
- js
top:
categories:
- js
copright:
---
<!-- 已经写好的文章在对应的md文件头部添加top：{number}即可 数值越大优先级越高 -->
### 实用型
#### 获取url查询参数
```javascript
let q = {};
location.search.replace(/([^?&=]+)=([^&]+)/g,(_,k,v)=>q[k] = v);
console.log(q)
``` 
#### reduce用法
```javascript
// 统计数组中相同项的个数
var cars = ['BMW','Benz', 'Benz', 'Tesla', 'BMW', 'Toyota'];
var carsObj = cars.reduce(function (obj, name,index,arr) {
    // obj：初始值 后面定义的  name：循环的item值  index：索引  arr：原数组
    obj[name] = obj[name] ? ++obj[name] : 1;
    return obj;
}, {});//定义初始值
```
#### 生成UUID
```javascript
getUUID(){
    var chars = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'.split('');
    var uuid = [];
    var i;
    var r;
    for (i = 0; i < 32; i++) {
        if (!uuid[i]) {
            r = 0 | Math.random()*16;
            uuid[i] = chars[(i == 19) ? (r & 0x3) | 0x8 : r];
        }
    }
    return uuid.join('');
}
```

#### 获取过去多少天的日历数组
```javascript
let days = [...Array(7).keys()].map(days => new Date(Date.now() - 86400000 * days))
```

#### 字符窜比较时间先后
```javascript
var st = "2014-08-08";
var et = "2014-09-09";
console.log(st>et, st<et); // false true
console.log("21:00"<"09:10");  // false
console.log("21:00">"09:10");   // true   时间形式注意补0
```

#### 获取当前时间并每秒刷新
```javascript
setInterval(()=>document.body.innerHTML=new Date().toLocaleString().slice(10,19))
```

#### 打乱数组
```javascript
let a = (arr) => arr.slice().sort(() => Math.random() - 0.5)
let b = a([1,2,3,4,5,6,7,8,9])
```

#### 生成随机颜色
```javascript
console.log('#' + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, '0'));
//#c618b2
```

#### 数组去重
```javascript
let arr1 = ["ces","ces",3,"你好","你好",1,2];
let newArr = [...new Set(arr1)]
```

#### 创建特定大小的数组
```javascript
let arr = [...Array(3).keys()] //[0,1,2]
```

#### 保留两位小数，不足补零
```javascript
function addZ(num,len=2){ 
    num=num.toString() 
    if(!num.includes('.')) num=num+'.' 
    if(!Object.is(Number(num),NaN)){ 
        num= Number(num).toFixed(len)   
        
    } 
    else if(num.includes(',')){ 
        if(!Object.is(Number(num.replace(/,/g,'')),NaN)){ 
        len=num.indexOf('.')+Number(len)+1 
        num= num.length<len? num.padEnd(len,'0'):num; 
        } 
    } 
    return num 
} 
console.log(addZ(num,3))
```
### 了解型（可读性比较差）
#### 取整 类似于parseInt
```javascript
~~ 3.14159 //3
3.14159 >> 0 //3
3.14159 | 0 //3
```

#### 比较值是否相等 会将字符窜转化为数字，相等返回0， 不相等返回两个的和
```javascript
('2' ^ '22') //24
('2' ^ '呵') //2
('33' ^ '33') //0
```

#### 强制转化为数字 
```javascript
'32' * 1 // * 1 转化
 console.log(+ '123') // + 转化
```

#### 四舍五入精确到指定小数点位数
```javascript
let num = 129;
const round = (n, decimals = 0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`)
```

#### 数字补零
```javascript
const addZero1 = (num, len = 2) => (`0${num}`).slice(-len)
const addZero2 = (num, len = 2) => (`${num}`).padStart( len   , '0')
addZero1(3) // 03
addZero2(32,4)  // 0032
```


