---
date: 2020/11/10 09:53:44
updated: 2020/11/10 09:53:44
categories:
- css
tags:
- html
- css
---

### 业务场景

同事直接在UI上搬运代码，整体页面偏小，div的id和css杂乱不堪，几乎无法修改，我又不想重写一遍

### 解决方案

使用CSS3解决：

```css
body {
      /*设置或检索对象的缩放比例*/
      zoom: 1.2;
      /*scale 改变的是坐标轴的刻度而不是宽高*/
      transform: scale(1.2);
      transform-origin: 0 0;
    }
```




