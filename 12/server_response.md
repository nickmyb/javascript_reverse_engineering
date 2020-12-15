server response
===============

## 0. Node顶层对象

```
// 需要使用 x = 1
var x = 1
console.log('x =', x)
console.log('global.x =', x)
```

## 1. 加解密方法搜索

```
// ==UserScript==
// @name         filter_decode_method
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

    for (var attr in window) {
        var lower_case_attr = attr.toLowerCase()
        if (lower_case_attr.indexOf('encode') != -1 || lower_case_attr.indexOf('encry') != -1) {
            console.log('encode or encry method found:\n', window[attr])
        }
        if (lower_case_attr.indexOf('decode') != -1 || lower_case_attr.indexOf('decry') != -1) {
            console.log('decode or decry method found:\n', window[attr])
        }
    }
})();
```


```
// ==UserScript==
// @name         hook_attr
// @namespace    basic
// @version      0.1
// @description  basic hook
// @author       nickmyb
// @include      *
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    function hook_attr(obj, attr) {
        let f = obj[attr]
        obj[attr] = function() {
            console.log('hooked: obj =', obj, 'attr =', attr)
            console.log('hooked: arguments =', arguments)
            let ret = f.apply(obj, arguments)
            console.log('ret =', ret)
            debugger
            return ret
        }
    }

    // base64
    hook_attr(JSON, 'parse')
})();
```