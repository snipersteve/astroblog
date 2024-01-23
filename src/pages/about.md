---
title: Hello World Again
layout: ../layouts/AboutLayout.astro
---

科学生活系列文章旨在把每一件事情都尽可能做到科学合理、尽可能自动化。自动化是我毕生追求，虽然我读的不是自动化专业，但对于自动化有着难以名状的执念，能减少一个手动步骤都是好的。不过这样一来也一定程度上把我兴趣领域局限在电脑方面，后面应该进一步拓展到生活的方方面面，这样才算是越活越明白。在科学系列文章开始前，首先把这个博客先弄弄好。

关于博客的折腾，从来没有停过，早年折腾过的博客不下20个，但写过的博文未必有20篇。在本次搭博客之前，是用一个github-action[方案](https://github.com/AlanDecode/Blog-With-GitHub-Boilerplate)跑的博客，完全自动，已经很科学了，但觉得编辑环节还不够可视化，于是计划改为在本地用Typora编辑，并把本地文同步到服务器上去用hexo生成静态网页。

首先要解决的是同步问题，用的是resiliosync。因为官方没有arm版docker，用了一个三年前更新的第三方版本，也能用。然后把这句命令挂后台 [^1]，就可以实现实时更新了，很方便。

```
nohup hexo generate --watch &
```

![后台运行持久化](https://snipersteve-public.oss-cn-hangzhou.aliyuncs.com/pic/assets/net-img-image-20220228145428676-20231130225932-8aza1qf.png)

不过，hexo的图片markdown语法和typora的不兼容（路径问题），搞了半天才让两者都能正常显示。需要装一个插件，改一行hexo的配置，并在typora里的相应设置图片保存路径。

```
npm install hexo-asset-image-for-hexo5 --save
post_asset_folder: true
```

至此就完成了，本地随意编辑markdown文件都会让网站自动更新，十分舒适。有点像我当年用bitcron [^2]时的感觉（dropbox同步模式）。

博客主题选了Fluid，配置多样，文档详细 [^3]，样式也很好看。

```
npm install hexo-theme-fluid --save
```

评论系统选了Cusdis [^4]，不要求强制留邮箱我很喜欢，轻量化还支持自部署，暂且用官方的。

![hexo](https://snipersteve-public.oss-cn-hangzhou.aliyuncs.com/pic/assets/net-img-image-20220401145543819-20231130225933-wjcn6cm.png)

---

update 考虑到resilio的同步有时反应迟缓，改为跑webdav（ugeek/webdav:arm），直接编辑服务器上文件，只是在没网情况下就没法写了。两种方案各有优劣。

由于oracle主机缓慢的连接速度，图片就不放在上面了，改为走图床，所以也不用再去处理复杂的图片路径问题了。同时得益于typora非常方便的picgo插件，可以实现在typora中粘贴图片，自动上传至github仓库并返回套了jsdelivr cdn的链接。这样我的post文件夹就变得很干净，只有几个markdown文件，通用性也更广。

给老婆也跑了一个博客，内容放在notion里，然后部署在vercel上，非常无脑，效果也不错。

---

update 瞎逛看到有一个叫hugo的静态博客程序，特点是编译速度快，而且生成博客不需要额外命令。这可太适合我了，赶紧用docker跑了一个，随便选了个主题，很快就把博客搭起来了。把markdown文件全部丢进去，博客迁移就完成了。主题配置微调了一下，非常简洁，访问速度也很快。博客的撰写或修改都只要编辑md文件就行，不需要别的任何操作，比hexo简单多了，十分满意。hexo的博客刚跑了没多久就下线有点可惜，还是在vercel上留了一份，留个念想。

![hugo](https://snipersteve-public.oss-cn-hangzhou.aliyuncs.com/pic/assets/net-img-image-20220401145816855-20231130225934-cndsls3.png)

---

update 主题换成zozo，这个主题真是一眼就喜欢，极致性冷淡，没有多余的色彩，布局也十分美观。

![zozo](https://snipersteve-public.oss-cn-hangzhou.aliyuncs.com/pic/assets/net-img-image-20220405225647502-20231130225935-9c6a1a6.png)

不过默认主题下图片不居中比较难受，于是参照网上的[一篇文章](https://www.zatp.com/post/hugo-fancybox/)，通过 Markdown Render Hooks 的方式覆盖 Hugo 的 Markdown 渲染方式，进行了两处改动。

一是新增 layouts/*default/*markup/render-image.html 文件，内容如下：

```
<div class="post-img-view">
<a data-fancybox="gallery" href="{{ .Destination | safeURL }}">
<div align="center"><img src="{{ .Destination | safeURL }}" alt="{{ .Text }}"></div>
</a>
</div>
```

二是在 layouts/partials/header.html 中添加如下代码：

```
<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.css" />
<script src="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.js"></script>
```

另外，把评论系统换成Valine，部署比Cudis麻烦些，但不需要审核直接显示更适合我这种博客（本来就没人来评论）。这一波折腾好之后，我想暂时不会去改动了。

202308 甲骨文把我放博客的那台小鸡停了，搞了好久才重新开起来，赶紧把所有的博客markdown文件都保存下来。以前没有给小鸡备份，实在是心大。重新选博客的时，选择跑了typecho，很方便很傻瓜，静态博客拜拜。

202309 甲骨文把我整个帐号封了，垃圾。于是去买了racknerd非常便宜的小鸡。跑了1panel，这是一个非常好用的开源面板，完全不比宝塔差。在1panel的市场中，有一个其同厂开发的网站程序halo，完全也可以用来当博客。直接用了默认模板，很好看很有现代感。通过atom的方式把原来的博客全部导进来，总体很不错。

有了上次的教训，赶紧把数据库作个备份，每天自动上传OSS，很安心。然后把防火墙也开起来，以前用甲骨文的时候，端口全开过于豪放（这大约也是被封号的原因），这次收敛点。总体来说，这个小鸡性价比还可以，1G的内存，halo就吃掉近一半内存，其他随便跑点docker，就可以了。

202311 racknerd小机连接速度还是太慢了，最终还是决定移回国内，买了阿里小机。把域名做了个备案，花了10天时间。终于可以跑博客了，还是继续跑halo。索性又花了点时间，把所有本地图片、github上的图片全部移到了oss上，还是oss管理最为方便。一堆纯文本的markdown文件，基本就是博客的全部了。而且halo的编辑器里，也有直接保存图片到oss的插件，很不错。

![img](https://snipersteve-public.oss-cn-hangzhou.aliyuncs.com/pic/2023/11/30%20/image-wmmqkidz.png)

---

[^1]: 在当shell中提示了nohup成功后，还需要按终端上键盘任意键退回到shell输入命令窗口，然后通过在shell中输入exit来退出终端；如果在nohup执行成功后直接点关闭程序按钮关闭终端的话，这时候会断掉该命令所对应的session，导致nohup对应的进程被通知需要一起shutdown，起不到关掉终端后调用程序继续后台运行的作用。
[^2]: 一个已经声明关闭的服务，但网站还在 https://us.bitcron.com/
[^3]: 国人开发的hexo主题 https://fluid-dev.github.io/hexo-fluid-docs/
[^4]: 国人开发的轻量化评论系统 https://cusdis.com/
