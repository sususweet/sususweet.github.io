---
layout: post
title: Dify 插件离线打包
categories: [web]
description: Dify 插件离线打包
keywords: dify
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

## Dify 插件离线打包

![](https://i-blog.csdnimg.cn/direct/c5129ce393ad47df95de5d3d85e1aa53.png)

直接下载这个没用，这个相当于只是一个目录，里面很多内容和依赖，需要一起打包。具体过程如下。

先把目录下载下来在 外网 机将插件相关依赖都打包好。关键是dify-plugin模块，

![](https://i-blog.csdnimg.cn/direct/e512459ba3714d0e836695b3bfbba3d1.png)

下载dify-plugin-repackagin g代码

项目地址：https://github.com/junjiem/dify-plugin-repackaging

解压拷贝到`dify-plugin-daemon`容器里面。

    /app/storage/plugin_packages

将langgenius-ollama\_0.1.2.difypkg下载后，拷贝到此镜像里面/app/storage/plugin\_packages/dify-plugin-repackaging

进入此容器

    docker exec -it 09 /bin/bashcd /app/storage/plugin_packages/dify-plugin-repackaging

安装好unzip

    apt update && apt install -y unzip

最终执行

    bash ./plugin_repackaging.sh local langgenius-ollama_0.1.2.difypkg

出现成功的标志

![](https://i-blog.csdnimg.cn/direct/682f94ac0ea24f2689ab89ffb1b058b4.png)

会生成offline，拷出来

![](https://i-blog.csdnimg.cn/direct/82f6d5317f9e48fc97a3194457278ef5.png)

放到内网，用网页，直接内网界面安装即可。

## Dify平台放开限制

- your .env configuration file: Change `FORCE_VERIFYING_SIGNATURE` to `false` , the Dify platform will allow the installation of all plugins that are not listed in the Dify Marketplace.

- your .env configuration file: Change `PLUGIN_MAX_PACKAGE_SIZE` to `524288000` , and the Dify platform will allow the installation of plug-ins within 500M.

- your .env configuration file: Change `NGINX_CLIENT_MAX_BODY_SIZE` to `500M` , and the Nginx client will allow uploading content up to 500M in size.

- 在 .env 配置文件将 `FORCE_VERIFYING_SIGNATURE` 改为 `false` ，Dify 平台将允许安装所有未在 Dify Marketplace 上架（审核）的插件。

- 在 .env 配置文件将 `PLUGIN_MAX_PACKAGE_SIZE` 增大为 `524288000`，Dify 平台将允许安装 500M 大小以内的插件。

- 在 .env 配置文件将 `NGINX_CLIENT_MAX_BODY_SIZE` 增大为 `500M`，Nginx客户端将允许上传 500M 大小以内的内容。

## 通过本地安装插件

Visit the Dify platform's plugin management page, choose Local Package File to complete installation.

访问 Dify 平台的插件管理页，选择通过本地插件完成安装。

## 致谢

- [dify-插件重新打包](https://github.com/junjiem/dify-plugin-repackaging.git)
