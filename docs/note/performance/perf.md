## 性能

从两方面思考 `web` 的性能问题：**页面纬度**、**运行纬度**

* **页面纬度：**页面的加载所经历的过程，思考在这个过程中有哪些是需要重点关注并能做一些优化的事情。
* **运行维度：**通常指动画(js动画、css动画、其他动画等等)，以及交互性能问题等。

#### 需要深入思考的问题

* 页面到底是什么时间被渲染出来？

答：？？？

* 如何测量、上报、提升性能？

答：如下图所示：

<img src="/asset/img/perf.jpeg" width="700"/>

开发者通常通过 `DevTools` 来分析，用 `WebPerf APIS` 来获取和上报数据。

对于上报，开源的工具可以使用 [grafana](https://github.com/grafana/grafana)，或者使用公司自建的上报系统。



#### 与性能相关的资源链接

* [w3c 性能工作小组](https://github.com/w3c/web-performance)

`w3c 性能工作小组` 会制定相应的指标规范，以及各个指标的意义。浏览器会根据该规范收集数据并暴露给开发者，开发者需要做的就是获取浏览器所暴露出来的信息。

* [我理解的前端性能 & 优化](https://zhuanlan.zhihu.com/p/33825610)

* [Why First Paint as Web Perf API?](https://docs.google.com/document/d/1wdxSXo_jctZjdPaJeTtYYFF-rLtUFxrU72_7h9qbQaM/edit)

## 好东西

* [grafana](https://github.com/grafana/grafana)