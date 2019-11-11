---
title: "将Hexo博客部署至Netlify"
date: "2019-10-06T23:04:22+08:00"
tags: ["Netlify", "Hexo"]
discripion: "将基于Hexo搭建的个人博客部署到Netlify中。"
keywords: ["Netlify", "Hexo", "个人博客"]
categories: ["Tech"]
slug: "Hexo-deploy-Netlify"
---

## 前言
其实这个博客总共也才断断续续捣鼓了fortnight，被好奇心驱使着到处搜集相关资料，浏览研究了很多“巨人”的文章和令人惊艳的个人博客。最开始是按[Hexo](https://hexo.io/zh-cn/docs/)的文档将博客部署在GitHub Pages上的，但是这个国庆让我感受到了“墙”深深的恶意——几乎所有的科学上网服务器节点都失效了，访问GitHub速度极慢，所以部署于GitHub Pages的博客加载速度就非常慢(之前也快不到哪儿去)。然后了解到了Netlify的一些**特性**，决定将博客部署到Netlify上。
## Netlify简介
[Netlify](https://www.netlify.com/)是一个静态网站自动化部署系统，可快速将静态网站直接构建部署，不需要你提供静态资源服务器。Netlify基于JAMstack架构，即客户端JavaScript、可重用API和预构建Markup，这是一种现代化、未来化的构建网站的方法，可以提供更好的性能([全球CDN](https://www.netlify.com/blog/2016/04/15/make-your-site-faster-with-netlifys-intelligent-cdn/))、安全性(HTTPS)和更好的开发体验(持续部署及Git集成)。且Netlify提供的功能和服务相比GitHub Pages可太强大了😏——绑定域名、页面重定向、静态资源文件压缩、免费的HTTPS等等，而且[文档](https://www.netlify.com/docs/)翻译过来也比较清晰流畅…
### 优势
- Simply Push to Deploy

    支持热部署，只需要将代码push到Git远程仓库即可自动构建及更新。

- Free automatic HTTPS

    支持自定义域名，提供免费HTTPS，可上传域名证书。

- Never have to leave Terminal

    可在终端中操作Netlify。

## 部署步骤

虽然Netlify没有中文文档，但也别被它的全英文唬住，部署的步骤还是比较简单的，但是它提供的强大功能我也并没有尝试很多，还在持续摸索总结中。

Netlify支持两种部署方式，直接拖拽站点文件至页面部署和**连接Git方式的持续部署**，因为之前博客是将Hexo生成的`public`文件夹部署至GitHub Pages的，为了写博客的便利性我还是选择采用Git方式部署，这样部署成功后还是可以本地`hexo g`➡️`hexo d`实现博客的部署更新。

首先直接使用GitHub账号登录Netlify，点击界面中的`New site from Git`按钮：

![new_site.png](http://blog.xuezenghui.com/deploy_to_netlify/new_site.png "New site from Git")


第一步，选择连接GitHub：

![step1.png](http://blog.xuezenghui.com/deploy_to_netlify/step1.png "连接GitHub")


> 需要注意的是进行此步骤时尽量关闭所有的科学上网工具，安安分分使用内网，否则..或将..导致网络请求无法连接GitHub，从而迟迟不跳出授权的页面。鬼知道我当时费了多少周折寻求解决办法结果花费一秒的时间关掉Shadowsocks就成了？？？🙃

授权，直接按照默认的选项点击`Install`即可：

![step1.1.png](http://blog.xuezenghui.com/deploy_to_netlify/step1.1.png "GitHub授权")


第二步，选择博客的仓库，我这里为Xuezenghuigithub.github.io仓库：

![step2.png](http://blog.xuezenghui.com/deploy_to_netlify/step2.png "选择仓库")

第三步，配置选项、构建部署：

![step3.png](http://blog.xuezenghui.com/deploy_to_netlify/step3.png "构建部署")

接着等待片刻就部署成功了🍺，点击笑脸选项卡即可访问部署成功的网站：

![deploy_success.png](http://blog.xuezenghui.com/deploy_to_netlify/deploy_success.png "部署成功")

### 绑定域名，设置DNS
首先肯定得申请域名了，很多前辈推荐狗爹[GoDaddy](https://sg.godaddy.com/zh)上购买域名，言之划算且不需要繁琐的域名备案，但是折腾了一番发现中国内地在GoDaddy上购买`.com`域名是不能使用支付宝支付的，其它支付方式又很麻烦，故弃之选择在[阿里云](https://www.aliyun.com/)上购买域名，操作方便价格无差——真香！(叮！支付宝到账0.5元！)


有了域名后点击部署项目的Domain setting，在Custom domains内直接输入域名即可，然后去DNS设置面板：

![domains.png](http://blog.xuezenghui.com/deploy_to_netlify/domains.png "DNS设置入口")

按照面板内步骤在阿里云域名控制台修改域名的DNS服务器，修改成功后域名后就有了上图中绿色的`Netlify DNS`标志，这时域名就由Netlify管理了，DNS解析也会自动配置，省事ing

![DNS_server.png](http://blog.xuezenghui.com/deploy_to_netlify/DNS_server.png "域名控制台截图")

### 设置HTTPS
上面说到了，Netlify是支持免费HTTPS的，只需要在Domain setting的HTTPS设置中点击按钮即可为网站添加Netlify提供的免费的[Let's Encrypt](https://letsencrypt.org/)证书，当然，阿里云也提供了免费的SSL证书，不嫌麻烦的话也可以使用自定义的证书，但本质上都是为了域名前的那个🔒，区别不大。

![HTTPS.png](http://blog.xuezenghui.com/deploy_to_netlify/HTTPS.png "HTTPS")

**要说明的是**，虽然已经有了HTTPS，但访问博客后域名前是❗️而不是🔒，提示`与网站之间建立的连接并非完全安全`，这是因为网站内包含了其它的非HTTP请求，比如七牛云的图片外链等，解决办法就是将博客文章内的HTTP链接全部替换为HTTPS，但说回来这种情况也**并不影响正常的浏览访问**，而且我这个穷鬼不配拥有七牛云的HTTPS格式图片外链🌚。
### 修改博客相关配置
绑定了自己的域名后相关的功能会失效，比如Valine评论会不显示、百度统计获取不到新域名的网站报告等，需要对相关配置加以修改。
#### 博客配置
修改博客的URL配置：
```python
# 文件位置：blog/_config.yml

# URL
url: https://blog.xuezenghui.com
```
#### Valine评论系统配置
登入[LeanCloud](https://leancloud.cn/)，进入**设置**➡️**安全中心**➡️**Web安全域名**中添加域名：
```
https://xuezenghuigithub.github.io/
https://www.blog.xuezenghui.com/
http://www.blog.xuezenghui.com/
```
> 协议、域名、端口号必须完全一致，自己申请域名的`www`不能省略。

#### 百度统计配置
登录[百度统计](https://tongji.baidu.com/web/10000070711/welcome/login)，新增网站，获取代码，将代码中`hm.src`行`?`后的字符复制，修改配置：
```
# 文件位置：blog/_config.yml

# Baidu Analytics ID
baidu_analytics: 123hasdhda8d2u3dhiua
```