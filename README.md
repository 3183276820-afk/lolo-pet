# lolo 桌宠 🐻

一个会陪在你桌面上的小熊桌宠：支持提醒、番茄钟、面板、托盘菜单，桌面常驻不占地。

> 🌐 **国内打不开 github.com？** 任选其一，内容完全一样：
> - **换镜像域名**：把链接里的 `github.com` 换成 `kgithub.com`（例：`https://kgithub.com/bobobizzz/lolo-pet`），或换成 `gitclone.com/github.com/bobobizzz/lolo-pet`。
> - **用反代读文件**：教程 / 脚本等文件可直接用 `https://ghfast.top/https://raw.githubusercontent.com/bobobizzz/lolo-pet/main/<文件名>` 打开（仅文件，不支持网页）。

## 下载安装

### Windows 用户（最简单）
前往 [Releases](https://github.com/bobobizzz/lolo-pet/releases) 下载 `.exe` 安装包，双击按提示安装即可。
（下载卡住时用反代直链：在原始下载链接前拼 `https://ghfast.top/`，例如
`https://ghfast.top/https://github.com/bobobizzz/lolo-pet/releases/latest/download/lolo-win-setup.exe`）

### Mac 用户（需要走「源码运行」）
由于 macOS 会对未公证的软件拦截，Mac 版不提供直接安装包，请改用**源码运行**方式：

1. 打开「终端」（`Command + 空格` → 搜「终端」→ 回车），粘贴下面这一行并回车。脚本会自动完成下载源码、装依赖、生成桌面图标并启动：

   ```bash
   curl -sL https://ghfast.top/https://raw.githubusercontent.com/bobobizzz/lolo/main/install-mac.sh | bash
   ```

2. 完整图文步骤见 👉 [Mac 源码运行教程（反代直链，好打开）](https://ghfast.top/https://raw.githubusercontent.com/bobobizzz/lolo-pet/main/Mac-源码运行指引.md)
   （或在本 Release 附件里看 `Mac源码运行教程.md`）

> 首次打开 macOS 会弹「无法验证开发者」，属正常现象，按脚本引导放行一次即可，以后不再拦。

## 关于这两个仓库
- `bobobizzz/lolo-pet`（本仓库）—— **分发仓库**，只放打包好的安装包和说明，**不含源码**。
- `bobobizzz/lolo` —— **源码仓库**，Mac 用户跑源码、或想自己研究代码时从这里获取。

装不上或有疑问，先参考上面的 Mac 教程，或联系作者。
