[chrome devtools](https://developers.google.com/web/tools/chrome-devtools)
==========================================================================

## 1. 各个面板

## 2. Network

1. 关键词Filter
2. Preserve log & Disable cache
3. Search
4. Initiator
5. Chrome2Python: curl -> Postman Import -> Paste Raw Text -> Import -> Code -> Python
6. shift悬停请求: 红色为依赖于悬停请求的请求, 绿色为悬停请求的发起者

## 3. Source

-
    1. Filesystem加载修改本地文件
    2. Overrides代理修改远程代码: 添加本地文件夹 -> Save for overrides
    3. Content scrpit: 插件加载的JS
-
    1. Add conditional breakpoint
    2. Ctrl + F / Ctrl + Shift + F(工具栏右上角三点)

## 4. Console

1. copy(可以直接粘贴至编辑器)
2. $_: 上次计算结果, $:document.querySelector, $$: document.querySelectorAll, $x: XPATH
