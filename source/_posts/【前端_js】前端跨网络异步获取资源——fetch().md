---
title: '【前端_js】前端跨网络异步获取资源——fetch()'
date: 2019-06-13 07:40:00
categories:
  - 前端_js
------

[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 提供了一个 JavaScript接口，用于访问和操纵HTTP管道的部分，例如请求和响应。它还提供了一个全局 [`fetch()`](https://developer.mozilla.org/zh-CN/docs/Web/API/GlobalFetch/fetch "此页面仍未被本地化, 期待您的翻译!")方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。

这种功能以前是使用  [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest "使用XMLHttpRequest (XHR)对象可以与服务器交互。您可以从URL获取数据，而无需让整个的页面刷新。这使得Web页面可以只更新页面的局部，而不影响用户的操作。XMLHttpRequest在 Ajax 编程中被大量使用。")实现的。Fetch提供了一个更好的替代方法，可以很容易地被其他技术使用，例如 [`Service Workers`](https://developer.mozilla.org/zh-CN/docs/Web/API/ServiceWorker_API "此页面仍未被本地化, 期待您的翻译!")。Fetch还提供了单个逻辑位置来定义其他HTTP相关概念，例如CORS和HTTP的扩展。

[使用 Fetch](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch)
