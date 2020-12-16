CSS Anti-Crawler
================

## 0. CSS反爬

关键在于理解CSS反爬方案的原理实现

## 1. 字体反爬

- 原理
    - 利用font-family,响应中为UNICODE字符,利用CSS映射到可读字体
- 解决方案
    - 下载 woff 字体文件转化为 tff 文件
    - 用百度字体编辑器打开 tff 文件并确定 unicode 与实际值的映射关系
    - 将下载的 HTML 内容按照映射关系替换
    - 有些网站会动态生成 woff，这种反爬措施比较难以自动化绕开

## 2. 背景反爬

- 原理
    - 利用雪碧图加偏移展示
- 解决方案
    - 下载雪碧图手动检查其对应的值
    - 在调试工具中调整 background-position 的偏移量找到各偏移量与实际值的映射关系

## 2. 伪类反爬

- 原理
    - 通过伪类的 content 属性设置显示的内容
- 解决方案
    - 自动化测试工具
    - const ele = document.querySelector('.classname')
    - const styles = getComputedStyle(el, 'before')
    - console.log(styles.content)

## 3. 元素定位反爬

- 原理
    - 利用绝对定位(position: absolute)将某一个数字或字符将原数字或字符通过一定的偏移量替换
- 解决方案
    - 计算元素偏移量,还原真实值

## 4. 字符分割反爬

- 原理
    - 字符串用多个tag分割,使用inline-block单行显示,还混淆带有display:none的标签
- 解决方案
    - 过滤出有效tag然后去除display:none的部分
