加密的快速定位
============

## 1. 基础分析

1. 全局搜索
2. 调用栈
3. XHR断点
4. DOM/EVENT断点

## 2. 快速定位

### 2.1 需要快速定位的情况

1. 全局搜索无果
2. js生成cookies
3. get获得的属性如何定位set位置

### 2.2 用于快速定位的脚本

#### 2.2.1 JSON

```
// ==UserScript==
// @name         hook_JSON
// @namespace    basic
// @version      0.1
// @description  basic hook
// @author       nickmyb
// @include      *
// @run-at       document-start
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function hook_JSON() {
        var _stringify = JSON.stringify
        JSON.stringify = function(params) {
            console.log('hooked JSON.stringify: params =', params)
            return _stringify(params)
        }

        var _parse = JSON.parse
        JSON.parse = function(params) {
            console.log('hooked JSON.parse: params =', params)
            return _parse(params)
        }
    }

    hook_JSON()
})();
```

#### 2.2.2 cookies

```
// from lengyue
// 只能设置单个cookie
var cookie_cache = document.cookie;
Object.defineProperty(document, 'cookie', {
    get: function() {
        console.log('Getting cookie');
        return cookie_cache;
    },
    // 对于cookies的字段增加做拼接
    set: function(val) {
        console.log('Setting cookie', val);
        var cookie = val.split(";")[0];
        var ncookie = cookie.split("=");
        var flag = false;
        var cache = cookie_cache.split("; ");
        cache = cache.map(function(a){
            if (a.split("=")[0] === ncookie[0]){
                flag = true;
                return cookie;
            }
            return a;
        })
        cookie_cache = cache.join("; ");
        if (!flag){
            cookie_cache += "; " + cookie;
        }
        this._value = val;
        return cookie_cache;
    },
});
```

#### 2.2.3 window

```
// 可以hook到属性的属性,但首先需要设置两级属性
// 这个脚本应该可以再优化层级看看如何递归!!!
// 一级属性
var window_flag_1 = 'flag_1';
// 二级属性
var window_flag_2 = 'flag_2';

var key_value_map = {};
var window_value = window[window_flag_1];

Object.defineProperty(window, window_flag_1, {
    get: function() {
        console.log('Getting', window, window_flag_1, '=', window_value);
//        debugger;
        return window_value;
    },
    set: function(val) {
        console.log('Setting', window, window_flag_1, '=', val);
        debugger;
        window_value = val;
        // 这里还会调用两次 get
        key_value_map[window[window_flag_1]] = window_flag_1;
        set_obj_attr(window[window_flag_1], window_flag_2);
    },
});

function set_obj_attr(obj, attr){
    var obj_attr_value = obj[attr];
    Object.defineProperty(obj, attr, {
        get: function() {
            console.log('Getting', key_value_map[obj], attr, '=', obj_attr_value);
//            debugger;
            return obj_attr_value;
        },
        set: function(val) {
            console.log('Setting', key_value_map[obj], attr, '=', val);
//            debugger;
            obj_attr_value = val;
        },
    });
}
```

#### 2.2.4 Function & eval

```
// from rocks
window.__cr_eval = window.eval
var myeval = function (src) {
    console.log(src);
    debugger;
    console.log("================ eval end ================")
    return window.__cr_eval(src)
}
var _myeval = myeval.bind(null)
_myeval.toString = window.__cr_eval.toString;
Object.defineProperty(window, 'eval', { value: _myeval })

window.__cr_fun = window.Function
var myfun = function () {
    var args = Array.prototype.slice.call(arguments, 0, -1).join(","), src = arguments[arguments.length - 1]
    console.log(src);
    console.log("================ Function end ================")
    return window.__cr_fun.apply(this, arguments)
}
myfun.toString = function() { return window.__cr_fun + "" } // 小花招
Object.defineProperty(window, 'Function', { value: myfun })
```

#### 2.2.5 websocket

```
WebSocket.prototype.__cr_send = WebSocket.prototype.send;
WebSocket.prototype.send = function(data) {
    console.info("Hook WebSocket", data);
    return this.__cr_send(data)
}
```

#### 2.2.6 其他

1. 多了解DevTools: 例如Log XMLHttpRequests / More Tools
2. 关注注入时机: Sources -> Event Listener Breakpoints -> Script
3. 视频中最后提到了瑞数的破解思路
