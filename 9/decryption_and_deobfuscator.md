Decryption & Deobfuscator
=========================

## 0. 小工具

- [格式化JS](https://beautifier.io/)
- Chrome设置: 禁用JavaScript

## 1. 一般分析流程

1. 查看关键包: 分析哪些参数是加密的
2. 搜索参数(参数名 / 参数名= / 参数名 = / 参数名: / 参数名 :)
3. 查看网络面板的 Initiator
4. XHR断点调试
5. HOOK相关逻辑
6. 分析加密
7. 补全加密逻辑

## 2. 常见混淆

### 2.1 颜文字或者火星文混淆

- 原理
    1. (0)["constructor"]["constructor"](code)()
    2. Function(code)()
    3. Function(p,a,c,k,e,r) 或者 function(p,a,c,k,e,d)
- 解决方案
    1. 复制代码到console执行,如果会报错的话可以从VM查看源代码
    2. 运行不报错的代码删除代码结尾的"(**args)" 或者 替换为".toString()"执行
    3. f(args)的 f 和 args分别复制到控制台执行, 一般情况f即eval
    4. eval(args) 可以直接 console.log(args)

### 2.2 obfuscator

- 原理
    1. 初始化数组
    2. 数组移位
    3. 解密函数

### 2.3 隐式 Style–CSS

- [隐式 Style–CSS](https://mp.weixin.qq.com/s/6GURR9mZzlt9rbiLCje7UA)
