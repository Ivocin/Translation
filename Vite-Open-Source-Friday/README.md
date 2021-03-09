# 【译】下一代前端工具 ViteJS 中英双语字幕 - Open Source Friday

> * 原视频地址：[Next generation frontend tooling with ViteJS ✨ Open Source Friday ✨](https://www.youtube.com/watch?v=UJypSr8IhKY)
> * 中英文字幕视频地址（B站）：[【译】下一代前端工具 ViteJS 中英双语字幕 - Open Source Friday](https://www.bilibili.com/video/BV1kh411Q7WN)
> * 中英文字幕视频地址（腾讯视频）：[【译】下一代前端工具 ViteJS 中英双语字幕 - Open Source Friday](https://v.qq.com/x/page/z3232ldhxho.html)
> * 英文字幕：听译、YouTube 自动字幕
> * 英文校对、翻译：[Ivocin](https://github.com/Ivocin)


关于 **Vite**，来看看作者本人怎么说。本视频是 [Vue](https://v3.vuejs.org/) 以及 [Vite](https://vitejs.dev/) 作者 [尤雨溪](https://github.com/yyx990803) 在 2021 年 2 月 12 日在 [Twitch](https://www.twitch.tv/) 上做客 [GitHub Open Source Friday](https://www.youtube.com/watch?v=qXHiXSSD3Kc&list=PL0lo9MOBetEFmtstItnKlhJJVmMghxc0P) 节目的直播视频。在视频里有尤大关于 Vite 的各项功能的详细阐述、大神在线编码、在线 Debug、大佬 diss webpack 以及对 Vite 的哲学思考。本视频很长，接近 70 分钟，下面是视频摘录，大家可以选择自己感兴趣的点自行传送。强烈建议大家观看视频，里面有很多细节相信大家会有收获。视频地址：[【译】下一代前端工具 ViteJS 中英双语字幕 - Open Source Friday](https://www.bilibili.com/video/BV1kh411Q7WN)



## Vite 的发音问题

[视频传送 - 1:18](https://www.bilibili.com/video/BV1kh411Q7WN?t=1m18s)

有关 Vite 发音的灵魂拷问：既然 Vite 使用的是其法语发音，那为什么 Vue 不用它的法语发音呢？（大概是因为法语读音不好听吧）。尤大告诉我们，作者说怎么读那就怎么读吧。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3974d6fff96744f9bb275aa8afc8768b~tplv-k3u1fbpfcp-watermark.image)

> 个人认为 Vue 和 Vite 的文档堪称良心了，首先就交代自己名字的发音，让全球开发者统一认知。再来看 [Svelte](https://svelte.dev/)，别说发音了，至今拼写还记不住。

## Vite 是什么

[视频传送 - 2:33](https://www.bilibili.com/video/BV1kh411Q7WN?t=2m33s)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/17acdc1d760a48dcbfaea480e0baa61c~tplv-k3u1fbpfcp-watermark.image)

尤大自己也说，很难一句话描述清楚 Vite 到底是什么。主要原因可能是它主要包括两个部分，一个基于 [ESM](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 的利用 [esbuild](https://esbuild.github.io/) 的开发服务器，另一个部分是基于 [Rollup](http://rollupjs.org/guide/en/) 的配置化的打包器。当然还有很多其他强大的功能，但是已经超过一句话了。尤大说市面上最接近 Vite 的产品是 [Parcel](https://parceljs.org/)，但二者的实现原理完全不同。

## 为什么 Vite 在此刻出现

[视频传送 - 4:53](https://www.bilibili.com/video/BV1kh411Q7WN?t=4m53s)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cf5d21b97fb34d1b8651b9c60edda5fd~tplv-k3u1fbpfcp-watermark.image)

本质原因应该是大部分现代浏览器（除了 IE 11）已经对原生 ES 模块支持的很好了，而且新版的 Node 也支持 ESM 了。ESM 终于可以在不久的将来一统江湖。原生的就是香。

## 起步 Demo

[视频传送 - 7:05](https://www.bilibili.com/video/BV1kh411Q7WN?t=7m5s)

不使用 @vitejs/create-app，从 0 开始创建一个 Vite 工程 demo。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/da8646922b9e421d806504d7db1f9124~tplv-k3u1fbpfcp-watermark.image)

## 入口文件是 `index.html`

[视频传送 - 14:25](https://www.bilibili.com/video/BV1kh411Q7WN?t=14m25s)

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/139d114819ce4264a1e654021273a0ca~tplv-k3u1fbpfcp-watermark.image)

## Vite 是 Opinionated 的

[视频传送 - 17:08](https://www.bilibili.com/video/BV1kh411Q7WN?t=17m8s)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e14e3b24afaa441989f01d4da880158c~tplv-k3u1fbpfcp-watermark.image)

划重点，Vite 是 **Opinionated** 的，视频里多次展示了这块内容。

其实 opinionated 本来是个贬义词，是固执己见的意思，而用在计算机科学领域，又变成了一个绝对的褒义词，号称自己 opinionated 的工具通过约定保证了易用性，又提供了配置以保证不会丧失灵活性。Vite 中内置了大量最佳实践的约定，省去了繁琐的配置，保证前端开发者常用的功能都是开箱即用的。

关于 Opinionated 的译法可以参考 [掘金翻译计划的一个 PR]( https://github.com/xitu/gold-miner/pull/7984#issuecomment-782794534)，[Vite 中文文档的一个 PR](https://github.com/vitejs/docs-cn/pull/17#discussion_r578294432) 这两处的讨论。

那么问题来了，列出几个 **opinionated** 和 **unopinionated** 的软件。我先来：Opinionated 的有 [Vite](https://vitejs.dev/)、[Prettier](https://prettier.io/), Unopinionated 的比如 [webpack](https://webpack.js.org/)，当然 unopinionated 可不是好词，应该不会有人在官方文档里写自己是 unopinionated 的。

这段是关于 webpack 的，看大佬如何 diss webpack：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/76d0e66930de463799e600f83250390e~tplv-k3u1fbpfcp-watermark.image)


## Vue CLI 会迁移到 Vite 上吗

[视频传送 - 23:56](https://www.bilibili.com/video/BV1kh411Q7WN?t=23m56s)

暂时不会，目前依然是基于 webpack 的，但是最终肯定是会迁移到 Vite 上的。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/438a6c1073f24267bb55a66546f5169a~tplv-k3u1fbpfcp-watermark.image)


## Vite 是框架无关的

[视频传送 - 25:43](https://www.bilibili.com/video/BV1kh411Q7WN?t=25m43s)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9803c90153fd45fbaf2bd5da230f38ff~tplv-k3u1fbpfcp-watermark.image)

Vite 提供了定义得非常好的 JavaScript API，可以在更高层级使用，比如 [VitePress](https://vitepress.vuejs.org/)，它是 [VuePress](https://vuepress.vuejs.org/) 的孪生兄弟，基于 Vite 构建。

## Tailwind CSS + Vite 实战

[视频传送 - 27:07](https://www.bilibili.com/video/BV1kh411Q7WN?t=27m7s)

尤大在线编写 Tailwind 代码翻车。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4ae1cfe5e3824147a92fbd2ade78a070~tplv-k3u1fbpfcp-watermark.image)

主持人调侃，原来 Evan You 也需要 debug 啊。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6f54aafc0a71493c9ae01d41aad94d8a~tplv-k3u1fbpfcp-watermark.image)



## Vite + React 实战

[视频传送 - 35:30](https://www.bilibili.com/video/BV1kh411Q7WN?t=35m30s)


主持人调侃，我们在线围观尤雨溪写 React！

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e936557a4254457ba91e399cf5f3aef~tplv-k3u1fbpfcp-watermark.image)


## 关于 Esbuild —— “快”就一个字

[视频传送 - 38:24](https://www.bilibili.com/video/BV1kh411Q7WN?t=38m24s)

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9d24f3ee0fe842d987d02c692f75987e~tplv-k3u1fbpfcp-watermark.image)

Esbuild 是 Vite 为何如此快速的原因，它比传统 tsc 快 20-30 倍。Vite 用 esbuild 替代 Rollup 进行预打包，速度也非常快。

这里尤大透露了他的工作电脑，**搭载 M1 芯片的 ARM 架构的 Mac Book Pro**，遗憾的是，当时的 esbuild 还不支持 ARM 架构，但 Go 的最新版已经支持。没想到过了几天，esbuild 就发布了其支持 M1 芯片的版本，尤大在第一时间做了测试：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9448be4b9c04f6799ba4d34652420bf~tplv-k3u1fbpfcp-watermark.image)



## DX 是啥

[视频传送 - 47:36](https://www.bilibili.com/video/BV1kh411Q7WN?t=47m36s)

在视频翻译过程中，听到尤大说了 DX 一词，由于不知道是什么含义，反反复复听了好多遍，后来 Google 发现，原来 DX 是 Developer Experience 的意思，看来关爱开发者是有官方术语的，关于 DX 的解释可以参考 [What Is DX? (Developer Experience)](https://medium.com/swlh/what-is-dx-developer-experience-401a0e44a9d9)。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/21f10d56066440d795615c0ca0a2309b~tplv-k3u1fbpfcp-watermark.image)

Vite 利用其快速的特性，极大提升了开发者的体验，尤大直言，他就像被宠坏了的孩子，项目启动超过 1 秒，他就很难忍受了。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b496ce3aab984cfd964c808fb314482e~tplv-k3u1fbpfcp-watermark.image)

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b28c6e172df0423aae73bcb73fa33a20~tplv-k3u1fbpfcp-watermark.image)


## 关于 SSR

[视频传送 - 52:20](https://www.bilibili.com/video/BV1kh411Q7WN?t=52m20s)

SSR 目前还处于实验阶段，详见[官网文档](https://vitejs.dev/guide/ssr.html)。


## 关于 HMR

[视频传送 - 57:59](https://www.bilibili.com/video/BV1kh411Q7WN?t=57m59s)


Vite 真正解决了 HMR 速度与随着应用越来越大而越来越慢的问题。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ea26577f66ee4c9d8359fe6d090905c3~tplv-k3u1fbpfcp-watermark.image)


## 为啥生产模式不用 esbuild，不是更快吗？

[视频传送 - 65:31](https://www.bilibili.com/video/BV1kh411Q7WN?t=65m31s)

其实也想用，但是 esbuild 目前对生产包支持不够健壮，很多配置无法通过 esbuild 实现。所以目前而言，Rollup 是一个好选择，虽然远比 esbuild 慢。

另外，可以用 esbuild 作为压缩器，替代 terser，详见 [build.minify](https://vitejs.dev/config/#build-minify),这样会更快，但是包的体积可能会有 5% - 10% 左右的增长，看用户取舍。


## 后记

好久没有做这么大型视频的翻译了，上一次还是[ React Conf 2018 的翻译](https://juejin.cn/post/6844903726684061710)。本视频翻译从春节假期 2 月 15 日开始，开工后时间比较少，断断续续花了三周多时间。好在 GitHub 在 Twitch 视频失效后，视频上传到了 YouTube 上，利用其自动字幕功能，后期节省了很多时间。确实 YouTube 的语音转文字功能更为强大。如果发现字幕存在问题，欢迎在视频评论区留言。希望这个视频能够帮助到大家。