---
title: webpack动态加载
date: 2018-08-20 19:30:28
tags: 
- webpack
categories: 
- fe
toc: true
---
function strToObject(str) {
    var arr = str.replace(/\s|\xA0/g,"").substr(1,str.length-2).split(',');
    var obj = {};
    for (var i = 0, _len = arr.length; i < _len; i++) {
        var _subArr = arr[i].split(":");
        var _key = _subArr[0].replace(/\"/g, "").replace(/\'/g, "");
        var _value = _subArr[1].replace(/\"/g, "").replace(/\"]'/g, "");
        obj[_key] = _value;
    }
    console.log(obj);
    return obj;
}