---
title: 常用js通用逻辑片段汇总
toc: true
date: 2018-10-02 17:37:41
tags: 
- jsvascript
categories:
---
平时开发总会遇到一些通用问题解决的代码片段，每次遇到的时候还总会重复查找，整理个文档持续追加，算是给自己做个备份
### 类似json的字符串转换成为对象
近期有个前端小白朋友遇到一个问题，后端给吐出了一个长得很像json的字符串，但是不是标准的json,需要自己将其解析，为了解决问题写了如下代码
```javascript
function strToJsonObject(str) {
    var arr = str.replace(/\s|\xA0/g,"").substr(1,str.length-2).split(',');
    var obj = {};
    for (var i = 0, _len = arr.length; i < _len; i++){
        var _subArr = arr[i].split(":");
        var _key = _subArr[0].replace(/\"/g, "").replace(/\'/g, "");
        var _value = _subArr[1].replace(/\"/g, "").replace(/\'/g, "");
        obj[_key] = _value;
    }
    return obj;
}
//test{"key1":"value1
var _string = '{"key1":"value1","key2":"value2","key3":"{"key1":"value1"}"}';
var retObj = strToJsonObject(_string);
```
因为数据结构是单层的，所以以上解析已经满足，但是还需要有优化
1、单引号和双引号的替换会把中间的引号也替换掉，而本意是只替换最前一个和最后一个
2、没有考虑多层嵌套导致的问题
