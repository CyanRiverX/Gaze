<div align="center">

# Gaze

<img src="./assets/icon.png" width="96" style="border-radius:12px"/>

### 完全本地的 Android 漫画阅读器

![Platform](https://img.shields.io/badge/Platform-Android-3ddc84?logo=android&logoColor=white)
![minSdk](https://img.shields.io/badge/minSdk-24-orange)
![License](https://img.shields.io/badge/License-Apache--2.0-blue)
![No Internet](https://img.shields.io/badge/Permissions-No%20Internet-success)

[下载](https://github.com/CyanRiverX/Gaze/releases) · [更新日志](https://github.com/CyanRiverX/Gaze/releases) · [问题反馈](https://github.com/CyanRiverX/Gaze/issues)

</div>

---

## 项目简介

Gaze 是一个只在本机运行的漫画阅读器。**不需要账号，不需要服务器，不联网** —— 安装包里没有任何网络权限。

它解决的核心问题是大压缩包：**读第 50 页就只解压第 50 页**，几百 MB 的合集不必先展开到缓存，翻开就能看。

CBZ / CBR / CBT / CB7、ZIP / RAR / TAR / 7Z、PDF、EPUB 都可以直接加入书架。

## 屏幕截图

|                   书架                   |                  标签                  |                      设置                      |                    阅读                    |
| :--------------------------------------: | :------------------------------------: | :--------------------------------------------: | :----------------------------------------: |
| ![shelf](./assets/screenshots/shelf.jpg) | ![tags](./assets/screenshots/tags.jpg) | ![settings](./assets/screenshots/settings.jpg) | ![reader](./assets/screenshots/reader.jpg) |

## 功能

**读取**

- 按需解压：归档层只回答「有哪些页」「某一页的流怎么拿到」，上层不碰整包
- 扩展名和内容对不上时按文件头魔数兜底（把 RAR 改名成 `.cbz` 照样能读）
- 自然数字排序：`1 / 2 / 3 / 10 / 11` 按数值排，`001`、`Page 1` 这类写法也认

**导入**

- 通过 Storage Access Framework 授权文件夹，可授权多个，自动扫描
- 增量扫描：按「路径 + 大小 + 修改时间」判定增改删，没变过的文件不重复解析；后台执行、显示进度、可取消
- 扫描与封面分离：先入库、再慢慢补封面，打开应用立刻就能看到书架
- 文件健康检查：逐个读归档中央目录，找出打不开、没有图片页、页数对不上的漫画，只报告不改数据

**阅读**

- 单页 / 双页 / 纵向连续三种模式
- 点击翻页：左右或上下三分之一翻页，中间切换控件栏，抬手即翻不陪等双击判定，可关
- 双击、双指、拖动缩放，四种适配（屏幕 / 宽度 / 高度 / 原比例）
- 按原图分辨率渲染：放大只解可见区域；超长条漫分块只为省内存，不降分辨率
- 坏页可重试或跳过，失败原因分流显示（加密 / 权限失效 / 文件消失 / 结构损坏 / 格式不符）
- PDF 按屏幕像素重新光栅化，放多大都不糊；EPUB 支持目录跳转与页码直达

**整理**

- 四种视图：全部 / 收藏 / 文件夹 / 标签；四种排版：2 列 / 3 列 / 小封面列表 / 大封面列表
- 按子文件夹分类（可选）：一个根目录下的每个子目录各自成为一个书架文件夹，多层依次保留为「黑白漫画 / 2024」
- 长按批量选择，批量收藏 / 打标签 / 移除；移除有 5 秒撤销窗口
- 详情页更换封面：从书里挑一页、用本机图片，或恢复自动生成
- 标签自建、可改名改色、长按拖动排序；收藏是独立概念，不依赖标签

**数据**

- 自动保存页码与进度；阅读偏好与翻页进度分开落库，改默认值不会被旧书固化
- 备份只含元数据（进度、统计、标签、收藏、封面、设置），`.gaze-backup` 体积很小，可从文件管理器「打开方式」直接导入
- 恢复按「内容指纹 → 采样哈希 → 文件名 + 大小」三级匹配，换机后文件位置全变了也能接回
- 媒体匹配：没找到文件的漫画集中列出，可手动重新定位或按内容自动接回，进度与标签都还在
- 自动备份按天 / 周 / 月，只在打开应用时检查，只轮转 `gaze-auto-` 开头的备份

**外观**

- 7 套纸张主题（宣纸 / 米黄 / 灰蓝 / 青瓷 / 藕粉 / 石墨 / 紫檀）+ 自定义主色
- 阅读器背景（黑 / 深灰 / 白）与系统主题解耦
- 渲染两档：优化渲染走分块解码省内存，传统渲染一次解整页以兼容个别异常图片

## 隐私

**不收集任何数据，也没有任何联网能力。**

- 未声明 `INTERNET` 权限；构建时还会显式删掉依赖可能传递进来的 `INTERNET` 与 `ACCESS_NETWORK_STATE`
- 没有账号、没有统计 SDK、没有广告、没有崩溃上报
- 所有数据只存在本机；文件访问只通过系统授权，可随时撤销
- 可以自己核对：`aapt2 dump badging Gaze.apk`，权限表里只有一条应用私有的 `DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION`

## 技术栈

| 领域 | 选型 |
|---|---|
| 语言 | Kotlin 2.1.20 |
| UI | Jetpack Compose + Material 3 |
| 归档解析 | commons-compress 1.27.1 + xz 1.10（ZIP / TAR / 7Z）、junrar 7.5.5（RAR） |
| PDF | Android `PdfRenderer` |
| 图片 | Coil 2.7.0 + `BitmapRegionDecoder`（区域解码） |
| 本地存储 | Room 2.7.1 |
| 设置 | DataStore Preferences |
| 文件访问 | Storage Access Framework |
| 构建 | AGP 8.11.1 + Gradle 8.13 + JDK 17 |

## 兼容性

| 项 | 值 |
|---|---|
| 最低版本 | Android 7.0（API 24） |
| 目标版本 | Android 16（API 36） |
| CPU 架构 | arm64-v8a |
| 签名 | APK Signature Scheme V2 + V3 |

只打包 `arm64-v8a`，32 位设备装不上。

## 项目结构

```
Gaze/
├── app/src/main/java/com/gaze/reader/
│   ├── archive/         归档读取层（ZIP / RAR / 7Z / TAR）+ 页面提供者 + PDF / EPUB
│   ├── backup/          备份导出与恢复
│   ├── core/            自然排序、文件名解析、格式定义、枚举
│   ├── data/            Room 数据库、仓库层、DataStore 设置
│   ├── domain/          标签配色、内容指纹、SAF 路径显示
│   ├── image/           图片解码、封面缓存
│   ├── scan/            增量扫描、文件重定位、封面生成与更换、SAF 权限
│   └── ui/              Compose 界面（导航 / 组件 / 页面 / 主题）
├── assets/              README 的图标与屏幕截图
└── tools/               图标生成脚本
```

## 构建

需要 JDK 17 与 Android SDK（compileSdk 36 / build-tools 35.0.0）。

```bash
export JAVA_HOME="<JDK 17 路径>"
export ANDROID_HOME="<Android SDK 路径>"
./gradlew assembleRelease
```

产物在 `app/build/outputs/apk/release/app-release.apk`。仓库不含签名密钥，未配置也能构建（产出未签名 APK）；要发布自己的构建，在仓库根目录放 `keystore.properties`（已 gitignore）：

```properties
storeFile=../path/to/my-release.jks
storePassword=<你的密码>
keyAlias=my-key
keyPassword=<你的密码>
```

> 换密钥意味着旧版本无法覆盖升级，用户必须先卸载。

## 版本记录

完整变更见 [Releases](https://github.com/CyanRiverX/Gaze/releases)。

| 版本 | 说明 |
|---|---|
| 1.7.4 | 按子文件夹分类；修切换排序后跳位；默认主题改为灰蓝 |
| 1.7.3 | 点击翻页；详情页更换封面；标签详情页跟随书架排版 |
| 1.7.2 | 修标签拖动排序在交界处失控 |
| 1.7.1 | 修恢复备份后标签与进度丢失；扫描合并重复记录 |
| 1.7.0 | 媒体匹配页；恢复备份改为「待重新授权」；备份补全字段 |
| 1.6.0 | 标签拖动排序；修返回直接退出应用 |
| 1.5.1 | 修「打开方式」导入备份失败；收藏独立成区 |
| 1.5.0 | 标签页两列卡片；文件名解析重做 |
| 1.4.0 | PDF 与 EPUB 支持；可拖动滚动条与返回顶部；8 套主题 |
| 1.3.1 | 修收藏页闪退；设置页改为一栏一卡 |
| 1.3.0 | 按原图分辨率渲染；标签长按批量；细滚动条 |
| 1.2.0 | 设置拆二级页；纸张主题；备份与自动备份 |
| 1.1.0 | 阅读统计；坏页重试与跳过；扩展元数据 |
| 1.0.1 | 修封面蒙纱、顶栏重叠、扫描提示反复弹出 |
| 1.0.0 | 首个版本 |

## 免责声明

**本项目的代码与文档主要由 AI 生成**，功能设计、架构取舍与文案由人把关，绝大部分具体实现出自 AI。

- 不保证没有缺陷，主要流程在真机上跑过，但不等于穷尽验证
- 按「现状」提供，不附带任何担保；数据丢失等损失作者不承担责任
- 如果那是你唯一的一份收藏，请先自己备份再用
- 要二次分发或商用，请先自己读一遍代码，尤其是文件读写、存储权限与归档解析部分

发现问题欢迎在 [Issues](https://github.com/CyanRiverX/Gaze/issues) 反馈，但不承诺修复时限。

## 许可证

[Apache License 2.0](LICENSE)
