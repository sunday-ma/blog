---
title: AI i18n，两行js实现html全自动翻译。
published: 2025-05-12
tags:
  - npm
  - vue
  - js
  - translate
  - plugin
toc: false
lang: zh
---

## 介绍

  + [Github](https://github.com/xnx3/translate)

  + [官方文档](https://translate.zvo.cn/index.html)

## 使用示例

  ### 在浏览器中使用（html + js）

  ```html
  <!-- 增加某种语言切换的按钮。注意 ul上加了一个 class="ignore" 代表这块代码不会被翻译到 -->
  <ul class="ignore">
    <li><a href="javascript:translate.changeLanguage('english');">English</a></li>|
    <li><a href="javascript:translate.changeLanguage('chinese_simplified');">简体中文</a></li>|
    <li><a href="javascript:translate.changeLanguage('chinese_traditional');">繁體中文</a></li>
  </ul>

  <!-- 引入多语言切换的js -->
  <script src="https://cdn.staticfile.net/translate.js/3.12.0/translate.js"></script>
  <script>
    translate.selectLanguageTag.show = false; // 不出现的select的选择语言
    translate.service.use('client.edge'); // 设置机器翻译服务通道
    translate.ignore.class.push('ignore'); // 忽略翻译
    translate.listener.start() // 开启html页面变化的监控
    translate.execute(); // 执行翻译初始化操作
  </script>
  ```

  ### 在Vue中使用

  1. 安装

      ```shell
      npm i i18n-jsautotranslate
      ```

  2. 使用

      ```javascript
      import translate from 'i18n-jsautotranslate'

      translate.selectLanguageTag.show = false // 不出现的select的选择语言
      window.translate = translate // 方便审核元素用控制台调试
      translate.service.use('client.edge') // 设置机器翻译服务通道
      translate.whole.enableAll() // 整体翻译

      // 页面渲染完毕后触发执行 translate.execute();
      nextTick(() => {
        translate.execute() // 执行翻译初始化操作
        // vue 的 input 中的 placeholder 属性会在 nextTick 之后延迟渲染，而这个属性是没有别的方式来监听的，所以额外加一个定时器
        setTimeout(() => {
          translate.execute() // 执行翻译初始化操作
        }, 500)
        translate.listener.start() // 开启html页面变化的监控
      })

      onUpdated(() => {
        translate.execute() // 执行翻译初始化操作
      })
      ```

  3. 注意: 比如你vue页面中有个切换语言的按钮，点击后进行切换为某种语言，千万不要在vue页面中引入 translate.js ，这样会造成重复引入重复翻译， vue中使用时只需要前面加个windows就好了; 不是vue的情况正常使用是 `translate.changeLanguage('english')`; 而在vue代码中触发则是 `window.translate.changeLanguage('english')`;

完。
