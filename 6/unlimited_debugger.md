无限debugger
============

1. 禁用所有断点(Deactivate breakpoints)
2. 禁用某处断点(Never pause here)
3. 条件断点(Add conditional breakpoint)
4. 中间人替换特征字符串(例如将 debugger 替换为 'debugger')
5. reres替换本地修改过的文件

## 1. 重写关键函数

1. 重写在关键函数 声明后执行前: 置空 function f(){}, 重写后可以关闭dev tools后再次打开
2. 在(function(){}).constructor === Function为true的浏览器上, 出现无限debugger后, console输入: Function.prototype.constructor = function(){}
3. 加载页面延迟后Esc可以取消当前请求
4. (function_name + "").indexOf("native") == -1 说明function_name不是native函数已经被hook(重写toString可以绕过)

## 2. [ReRes](https://apppit6dcs05916.pc.xiaoe-tech.com/detail/i_5e720f8a7ad97_xoa7sXug/1?from=p_5d9eb71212cbe_Ckzdcjsp&type=6)

1. ReRes替换代码的时候建议使用原始代码不要使用格式化后的代码,否则可能会有问题
2. 新型debugger: (function(){}["constructor"]("debu" + "gger")["call"]("action"))