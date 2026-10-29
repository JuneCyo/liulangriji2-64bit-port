# 流浪日记2：大山深处 — 64 位安卓兼容版 / Windows 电脑版

基于《流浪日记2》原版 APK 0.402（`org.cocos2d.StrayDiary22`）恢复运行环境，提供适用于 ARM64 安卓手机的 APK 和 Windows 本地浏览器启动器。非官方兼容工程，原游戏版权归晓风游戏及原作者。

安卓通过系统 WebView 执行原始 JavaScript 和资源，**不依赖原版 32 位 ARM 库**，不包含重新编译的 ARM64 Cocos 原生引擎。本仓库提供打包后的原版脚本与资源、兼容宿主和构建代码，不是原作者的完整 Creator 编辑器工程。

## 下载与使用

在 [Releases](https://github.com/JuneCyo/liulangriji2-64bit-port/releases/latest) 下载：

| 文件 | 用途 |
| --- | --- |
| `StrayDiary2-Android-0.402-port.1.apk` | 手机安装包，Android 6.0 或以上，使用系统 WebView |
| `StrayDiary2-Windows.zip` | 完整电脑版；解压后运行其中的 EXE，推荐下载这个文件 |
| `StrayDiary2-Windows-Launcher.exe` | 单独启动器；需要和完整包中的 `web` 文件夹放在一起 |
| `SHA256SUMS.txt` | Release 附件的 SHA-256 校验值 |

手机桌面名称“流浪日记2兼容版”，包名 `local.straydiary2.port`，版本 `0.402-port.1`（40201）。与原版及上一作的兼容版独立安装。已有本兼容版时直接覆盖更新可保留数据；卸载会删除应用本地存档。

电脑版需要 Windows .NET Framework 4.x 和现代浏览器，不需要安卓模拟器、Python 或 Node.js。启动器在 `127.0.0.1:18767` 提供本地游戏文件，托盘图标右键菜单可重新打开游戏或退出。游玩期间请保持启动器运行，并保留整个 `web` 文件夹。

顶部“存档 → 导出存档 / 导入存档”可通过 JSON 文件在手机和电脑之间手动转移进度，不提供云同步。电脑请固定使用同一浏览器的普通模式，清理网站数据会删除进度，建议定期导出。

本作是《流浪日记2：大山深处》0.402，与[上一作 1.21 的兼容工程](https://github.com/JuneCyo/liulangriji-64bit-port)不同。两作使用独立包名、电脑端口和存档格式，不能互相导入存档。本作存档键为 `userData`、`adoptData`、`dataCopy`。

## 当前版本

- 安卓 `0.402-port.1`（40201）：硬件加速、Canvas 2D、目标 60 FPS；初始化期间显示本作菜单静态预览，首帧呈现后接入真正菜单，预览期间按钮不可操作。
- 电脑保留浏览器默认渲染，两端维持原版 9:16 竖屏比例。
- 原版游戏逻辑、场景和图片保持不变；91 个原始文件的 SHA-256 见 `docs/original-assets.json`。

## 从代码构建

依赖 Windows、Python 3.10+、JDK 17、Android SDK API 35 和 Build Tools 35.0.0。运行命令时工作目录为仓库根目录。

```powershell
# 下载 Android 工具及固定版本的官方 Cocos 引擎源码到 .tools/
python tools/download_dependencies.py

# 制作正式 APK 和完整电脑版 ZIP，输出到 dist/
python tools/build_android.py
python tools/build_pc.py
```

仓库已包含可用 Web 引擎，普通打包无需 Node.js。重新编译引擎时需要 Node.js 18+ 和 npm：

```powershell
npm ci --prefix tools/js-build
node tools/build_web_engine.cjs
```

引擎采用官方 Cocos tag `1.9`（提交 `ada2855dbeb5c2913a4f44d2947854daea9aa3f5`），包含浏览器全屏兼容及宿主启动交接修改。原版 APK 使用 Cocos 1.8.2 的 32 位原生引擎。

首次构建会在 `.tools/signing/` 生成本地签名密钥，该目录不提交到仓库。自行构建的新签名无法覆盖安装 Release APK；继续更新同一自建应用时，必须保留自己的签名密钥、包名并递增版本号。

`tools/prepare_web.py` 可从自行准备的原始 APK 恢复资源，正常构建不需要再次提取。原始 APK、模拟器磁盘、个人存档和签名私钥不包含在公开代码或 Release 中。

## 验证范围

已验证启动、触控、新开局、天赋选择、开场剧情、山谷探索、攻击、逃跑、保存、继续游戏、存档导入导出及电脑和安卓之间的双向存档转移。已检查 APK 签名、ZIP 对齐、正式包关闭调试、91 个原始文件完整性，以及电脑版完整 ZIP 解压后的启动。详见 [验证记录](docs/validation.md)。

测试环境为 Windows + Chrome，以及 Android 12 / WebView 101 模拟器，安卓宿主运行架构为 x86_64。本作尚未在真我 GT Neo3 / Android 14 真机实测性能，未完整通关、覆盖所有结局或所有 Android 版本。60 FPS 是配置目标，实际帧率取决于设备和系统 WebView。

## 版权与许可

见 [NOTICE.md](NOTICE.md)、[web/NOTICE.txt](web/NOTICE.txt) 与 [web/licenses](web/licenses)。原游戏、引擎和第三方组件各自保留原版权与许可，公开兼容工程不改变原游戏的权利归属。
