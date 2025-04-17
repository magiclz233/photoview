<img src="./screenshots/photoview-logo.svg" height="92px" alt="photoview logo" />

[![License](https://img.shields.io/github/license/viktorstrate/photoview)](./LICENSE.txt)
[![GitHub contributors](https://img.shields.io/github/contributors/viktorstrate/photoview)](https://github.com/viktorstrate/photoview/graphs/contributors)
[![Docker Pulls](https://img.shields.io/docker/pulls/viktorstrate/photoview)](https://hub.docker.com/r/viktorstrate/photoview)
[![Docker builds](https://github.com/photoview/photoview/actions/workflows/build.yml/badge.svg?branch=master)](https://github.com/photoview/photoview/actions/workflows/build.yml)
[![codecov](https://codecov.io/gh/photoview/photoview/branch/master/graph/badge.svg?token=AATZKC93F7)](https://codecov.io/gh/photoview/photoview)

![screenshot](./screenshots/timeline.png)

Photoview 是一个简单且用户友好的照片库，专为摄影师设计，旨在提供一种简单快捷的方式来浏览包含数千张高分辨率照片的目录。

你可以配置 Photoview 在文件系统的指定目录中查找照片和视频。扫描器会自动获取你的媒体文件并开始生成缩略图，使浏览速度超快。

当你的媒体文件被扫描后，它们会按照文件系统中的相同组织方式显示在网站上。

> 如果你有关于设置或开发的问题，
> 欢迎加入 Discord 服务器 https://discord.gg/jQ392948u9

# Docker 用户注意事项！！！

我们已迁移到新的 Docker 镜像仓库 <https://hub.docker.com/r/photoview/photoview>，所有 `master` 标签的新镜像以及未来的发布版本都将在这里发布，而不是之前使用的镜像仓库。旧镜像仍可在旧仓库 <https://hub.docker.com/r/viktorstrate/photoview> 中访问，如果你想使用这些旧镜像，需要切换回旧仓库。

请更新你的 `docker-compose.yml` 文件以使用新仓库的 `photoview` 镜像，如相应的 compose 文件示例所示：<https://github.com/photoview/photoview/tree/master/docker-compose%20example>

## 演示站点

访问 https://photos.qpqp.dk/

用户名：**demo**

密码：**demo**

## 目录

- [Docker 用户注意事项！！！](#docker-用户注意事项)
  - [演示站点](#演示站点)
  - [目录](#目录)
  - [主要功能](#主要功能)
  - [支持的平台](#支持的平台)
  - [为什么又一个自托管照片库](#为什么又一个自托管照片库)
  - [入门 — 使用 Docker 设置](#入门--使用-docker-设置)
    - [初始设置](#初始设置)

## 主要功能

- **与文件系统紧密集成**。网站展示服务器本地文件系统中的图片；目录映射为相册。
- **用户管理**。每个用户都会创建一个本地文件系统路径，该路径下的照片可以被该用户访问。
- **分享功能**。相册和单个媒体文件都可以通过公共链接轻松分享，链接可以选择性地设置密码保护。
- **专为摄影设计**。Photoview 是为摄影师打造的，因此支持 **RAW** 文件格式和 **EXIF** 解析。
- **视频支持**。支持多种常见视频格式。视频将自动优化以适应网页播放。
- **人脸识别**。照片中的人脸将被自动检测，同一个人的照片将被分组在一起。
- **高性能**。自动生成缩略图，照片仅在屏幕上可见时才加载。在全屏模式下，在高分辨率图片完全加载之前显示缩略图。
- **安全性**。所有媒体资源都受 cookie-token 保护，所有密码都经过适当哈希处理，API 使用严格的 [CORS 策略](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。

## 支持的平台

- [Docker](https://hub.docker.com/r/viktorstrate/photoview/)
- [Arch Linux Aur](https://aur.archlinux.org/packages/photoview)
- [Unraid](https://forums.unraid.net/topic/103028-support-photoview-corneliousjd-repo/)
- EmbassyOS：[公告](https://start9labs.medium.com/new-service-photoview-72ee681b2ff0)，[仓库](https://github.com/Start9Labs/embassyos-photoview-wrapper)
- [YunoHost](https://github.com/YunoHost-Apps/photoview_ynh)

## 为什么又一个自托管照片库

已经存在很多开源的自托管照片库。这里列举几个：

- [Piwigo](https://github.com/Piwigo/Piwigo)
- [LibrePhotos](https://github.com/LibrePhotos/librephotos)
- [Photoprism](https://github.com/photoprism/photoprism)
- [Lychee](https://github.com/LycheeOrg/Lychee)

那为什么还要再开发一个呢？
我热爱摄影，我把所有照片都存储在本地文件服务器上。
这很棒，因为我可以直接在文件系统上组织照片，方便移动或备份。我希望能够控制照片的存储位置和方式。

然而，问题是即使在本地网络上，浏览 RAW 图像也非常麻烦。

我的服务器上存储了很多我想让家人也能访问的老家庭照片。
而且有些照片我想能够轻松地与其他人分享，而不需要他们先注册账号。

因此，我需要一个能够满足以下需求的解决方案：

- 基于扫描的方法，自动组织我的照片
- 支持 RAW 和 EXIF 解析
- 支持多用户，并能够分享相册和照片（包括公开分享）
- 简单直观且快速

所有照片库都能满足我的部分需求，但没有一个能完全满足所有需求。

## 入门 — 使用 Docker 设置

> 本节介绍如何在服务器上使用 Docker 运行 Photoview。
> 确保你的服务器上已安装并运行 Docker 和 docker-compose。
> 如果你想使用提供的 `Makefile`，还需要安装 `make`（参见步骤 4 了解详情）。
> 如果你想在备份场景中使用 7zz 而不是默认的 .tar.xz 格式，需要安装 `7zz`。阅读 `Makefile` 中 `backup` 部分顶部的注释了解更多详情。

1. 将 `docker-compose example` 文件夹的内容下载到服务器上你期望托管 Photoview 内部数据（数据库和缓存文件）的文件夹中。

   请注意，该文件夹包含 2 个版本的 docker-compose 文件：
   - `docker-compose.example.yml` - 功能完整且推荐用于大多数情况的配置
   - `docker-compose.minimal.example.yml` - 对于觉得前一个配置太复杂难以理解和管理的用户的最小化简单配置

   下载文件时，你只需要选择其中一个。
2. 重命名下载的文件并删除其名称中的 `example`（这样你需要有 `.env`、`docker-compose.yml` 和 `Makefile` 文件）。如果你在上一步选择了 `docker-compose.minimal.example.yml`，确保将其重命名为 `docker-compose.yml`。
3. 在文本编辑器中打开这些文件并阅读。根据文档注释的说明进行必要的修改，以适配你的设置。有两种类型的注释：以 `##` 开头的是解释和示例，不应取消注释；以 `#` 开头的是可选或替代配置部分，在相应解释中描述的特定情况下可以取消注释。最好按以下顺序查看文件：`.env`、`docker-compose.yml` 和 `Makefile`。
> 如果你的 `PGSQL_PASSWORD` 或 `MARIADB_PASSWORD` 包含特殊字符（如 `@`），确保进行 URL 编码。
4. 确保你的媒体库根文件夹及其所有文件和子文件夹对其他用户可读和可搜索：运行以下命令（或 `Makefile` 中的相应命令序列）：

   ```bash
   make readable
   ```

   如果命令返回 `Permission denied` 错误，请以拥有相应文件和文件夹的用户身份运行它们。或者，在命令前添加 `sudo `：这将切换到 `root` 用户执行上下文并要求输入 root 密码。你必须有权限在系统中运行 `sudo`。

   如果你不想给 `others` 组授予文件的必要权限，你可以选择：

   - 在主机上创建一个 GID=999 的组，并使 `photoview` 服务的卷内的所有文件和文件夹归这个组所有；然后为 `group` 部分设置适当的权限。
   - 在主机上创建一个 GID=999 的组和该组中 UID=999 的用户；然后将 `photoview` 服务的卷内的所有文件和文件夹的所有权更改为该用户；然后为 `user` 部分设置适当的权限。

   如果你配置了来自主机上其他位置的媒体文件的其他挂载（如 HOST_PHOTOVIEW_MEDIA_FAMILY 或其他），你需要手动为每个主机上的媒体根文件夹运行与 `Makefile` 中 `readable` 目标相同的命令：将每个命令复制到你的 shell 中，并将变量替换为不带尾部 `/` 的额外媒体根文件夹的绝对路径。为每个额外的根文件夹运行这两个命令。
5. 如果你的系统中没有安装 `make` 或不想将其用于 Photoview 管理活动，你可以直接在 shell 中使用 `Makefile` 中的相同命令，或创建自己的脚本。在这种情况下，确保先应用或替换 `.env` 中的变量。提供 `Makefile` 只是为了方便和简单，但它是可选的。
6. 通过运行以下命令启动服务器（或 `Makefile` 中的相应命令序列）：

   ```bash
   make all
   ```

如果 `docker-compose.yml` 文件中没有更改端点或端口，现在可以通过 http://localhost:8000 访问 Photoview

### 初始设置

如果一切设置正确，首次访问网站时你会看到初始设置向导。

![初始设置](./screenshots/initial-setup.png)

输入新的用户名和密码。

对于照片路径，输入 Docker 容器内照片所在的路径。
这可以在 `docker-compose.yml` 文件中的 `photoview` -> `volumes` 下设置。
默认位置是 `/photos`。

将创建一个新的管理员用户，可以访问初始设置中提供的路径下的照片。

在照片显示之前需要进行扫描，你可以通过导航到"设置