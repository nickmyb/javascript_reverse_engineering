调用javascript
==============

## 1. Base64(=)

## 2. MD5(32位英文数字)

## 3. Python调用JS

- PyMiniRacer(较新)
- PyExecJS

```
# 1. 安装Node.js
# 2. pip install pyexecjs

# 3. 自定义使用的引擎
# 长期
os.environ["EXECJS_RUNTIME"] = "Node"
# 暂时
import execjs.runtime_names
node = execjs.get(execjs.runtime_names.Node)

# 4. 查看当前调用环境
import execjs
execjs.get().name

# 5. 调用
# 如果需要在Python中使用JS中返回的object,在JS中JSON序列化
# ExecJS是通过命令行调用的JS,对于特殊字符会有些问题(解决方案: new上下文使用tempfile, 参数base64)
import execjs
javascript_text = '''
function sum(x, y) { 
    return x + y
}
'''
ctx = execjs.compile(javascript_text)
sum_1_2 = ctx.call("sum", 1, 2)
print(sum_1_2)
```

## 4. Selenium执行JS

```
ret = browser.execute_script(js)
```

## 5. Pyppeteer执行JS

```
# 调用结果selenium需要return, puppeteer不需要
ret = await page.evaluate(js, *data)

# 页面加载前调用,一般用于初始化环境
ret = await page.evaluateOnNewDocument(js, *data)
```

## 6. NodeJS执行JS

```
1. gRPC

2. JS HTTP(例如: Express)

# 缺少 window 时使用 {} / global代替 或者 jsdom
# Buffer.from(s).toString('base64') 代替 window.btoa
```