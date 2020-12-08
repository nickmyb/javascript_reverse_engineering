常用相关工具
==========

## 1. Charles

## 2. EditThisCookie

1. HttpOnly: JS无法获取

## 3. Toggle JavaScript

## 4. [Tampermonkey](https://www.tampermonkey.net/)

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
    hook_attr(window, 'btoa')
})();
```