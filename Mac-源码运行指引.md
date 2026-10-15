# lolo（Mac 版）源码运行指引

> 适用情形：Mac 上装不了 `.dmg`（被系统弹「已阻止恶意软件并移到废纸篓」、Gatekeeper 拦截、`xattr` 也放不了）时，改用**直接从源码启动**。
>
> **原理**：从终端用 Node 启动 Electron 进程，**不经过 `.app` 签名检查**，所以不会被 Gatekeeper 拦截，100% 能跑起来。

---

## 零、一行命令全自动（强烈推荐，先看这个）

打开「终端」（`Command + 空格` → 搜「终端」→ 回车），粘贴下面这一行并回车，剩下的一切由脚本自动完成：

```bash
curl -sL https://ghfast.top/https://raw.githubusercontent.com/bobobizzz/lolo/main/install-mac.sh | bash
```

**它会自动做**：检查 Node → 下载源码（走国内反代，不用挂梯子）→ 配置国内镜像 → 只装运行必需的依赖（跳过打包工具，省时间）→ 在桌面生成「启动lolo」图标 → 直接启动 lolo。

**装好以后**：桌面上双击「启动lolo」就能打开，以后再也不用碰终端。

> 如果提示没装 Node.js，脚本会自动打开下载页面；你装完 Node 后，**再跑一次上面那一行命令**即可。
>
> 想更新到最新版：终端执行 `rm -rf ~/lolo/lolo-main`，然后重跑那一行安装命令。

> **⚠ 首次打开几乎一定会被系统拦下（这是正常的）**：macOS 会弹「无法验证开发者」。因为 lolo 用的 Electron 没做苹果付费公证，系统只是例行盘问，解决只需一次：
> 1. 弹窗点「好」关掉；
> 2. 打开「系统设置 → 隐私与安全性 → 安全性」；
> 3. 找到 lolo / Electron 被拦截的记录，点右侧**「仍要打开」**；
> 4. 重新双击桌面「启动lolo」即可。
> 嫌麻烦也可终端执行 `sudo spctl --master-disable` 一劳永逸（用完可 `sudo spctl --master-enable` 恢复）。

只有这行命令**走不通**的时候，才需要看下面这些手动步骤。

---

## 一、优点与代价（先看这个再决定）

**优点**
- 彻底绕开所有苹果签名 / 公证 / Gatekeeper 拦截，一定能运行。
- 功能与正式版完全一样（桌宠、提醒、番茄钟、面板、托盘菜单都有）。

**代价**
- 需要在 Mac 上装 Node.js 环境（一次性的）。
- 第一次安装要下载 Electron（约 100–200 MB），需要网络，国内必须配镜像（文档已配好）。
- 以后想更新版本，要重新获取源码并 `npm install`（没有一键自动更新）。

> 如果你不是开发者、只是想用软件：**先用 dmg 那条路**；实在装不上再走本指引。

---

## 二、操作步骤

### 第 1 步：安装 Node.js（一次性）

1. 打开 https://nodejs.org ，点下载页面里的 **LTS 版本**（macOS 安装包 `.pkg`，选 Apple 芯片版本）。
2. 双击安装包，一路「继续 / 同意 / 安装」即可。
3. 装完后验证：打开「终端」，输入下面命令回车，能显示版本号就成功：

```bash
node -v
```

### 第 2 步：拿到 lolo 源码

二选一：

- **方式 A（最省事，推荐）**：让作者直接给你一个 **lolo 源码压缩包（zip）**，下载后解压到任意位置，比如「下载」文件夹里出现一个 `lolo` 文件夹。
- **方式 B（推荐，源码仓库是公开的，直接克隆即可）**：终端执行：

```bash
git clone https://github.com/bobobizzz/lolo.git
```

> ⚠️ **别搞混这两个仓库**：
> - `bobobizzz/lolo` —— **源码仓库**（公开），跑源码就从这里克隆（上面这条），里面有 `main.js` / `src` / `assets` 等完整代码。
> - `bobobizzz/lolo-pet` —— **分发仓库**（公开），只存放打包好的安装包和一个 README，**里面没有源码**，不能用来跑源码。你之前下载安装包用的链接（`.../lolo-pet/releases`）就是它。
>
> 若作者以后把源码仓库设为私有，方式 B 会失效（提示需要登录），此时改用**方式 A**（向作者要源码压缩包）。

> ⚠️ 源码目录里**必须**包含这几样（完整 zip 或 clone 都会有）：`src/`、`assets/`（内含 26 个动画 png）、`config/default-config.json`、`main.js`、`preload.js`、`package.json`。

### 第 3 步：配置国内镜像（关键！不配会卡住）

打开「终端」，**逐行**执行（每行回车）：

```bash
npm config set registry https://registry.npmmirror.com
```

```bash
export ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/
```

> 第二行的作用是让 Electron 本体从国内镜像下载，否则它会去 GitHub 拉，国内基本拉不动、会长时间卡住。
> ⚠️ 这条只对**当前这个终端窗口**有效，所以下面的安装步骤请**在同一个终端窗口里继续**，不要新开。

### 第 4 步：安装运行依赖

在**同一个终端窗口**里，先进入源码目录（比如源码在「下载」文件夹里）：

```bash
cd ~/Downloads/lolo
```

> 小技巧：输入 `cd `（cd 后面加一个空格）之后，直接把 `lolo` 文件夹**拖进终端窗口**，路径会自动填上，再回车。

然后安装：

```bash
npm install
```

这一步要下载 Electron，可能花几分钟，看到进度停下、重新出现命令提示符就完成了。

### 第 5 步：启动 lolo

```bash
npm start
```

看到 lolo 小熊出现就成功了 🎉

**以后每次想打开**（依赖已装好，不用重来）：只需开个终端执行

```bash
cd ~/Downloads/lolo && npm start
```

退出：在托盘图标上退出，或在终端按 `Ctrl + C`。

---

## 三、可选：做一个「双击就能启动」的小图标

不想每次开终端敲命令的话，可以自己做一个双击启动器：

1. 打开「文本编辑」（TextEdit），先按 `Command + Shift + T` 切换成**纯文本**模式。
2. 粘贴下面两行（第一行路径换成你自己的源码目录）：

```bash
cd ~/Downloads/lolo
npm start
```

3. 保存，文件名填 `启动lolo.command`，位置选「桌面」。
4. 在终端执行一次赋予权限：

```bash
chmod +x ~/Desktop/启动lolo.command
```

5. 以后**双击桌面上的「启动lolo」** 就能直接打开软件（首次可能问你要不要打开，选「打开」）。

> 这是你自己在本机创建的文件，不带「网上下载」标记，所以不会被 Gatekeeper 拦截。

---

## 四、常见问题

### 首次打开弹「无法验证开发者」或「已损坏，应移至废纸篓」？

这是**最正常、最常见**的情况，几乎所有没做苹果公证的软件都会这样。一次放行就能永久解决：

1. 弹窗里点「好」或「取消」关掉它。
2. 打开 **系统设置 → 隐私与安全性 → 安全性**。
3. 下方会出现一条「lolo」/「Electron」被拦截的提示，点它右侧的 **「仍要打开」**。
4. 再双击桌面「启动lolo」，这次就能正常打开，以后也不再拦。

> 嫌每次都烦、想一劳永逸：在终端执行 `sudo spctl --master-disable`，然后系统设置里把「允许以下来源」设为「任何来源」。用完后想恢复：`sudo spctl --master-enable`。

### 连 Electron 本体都被拦（「未打开 Electron.app，因其包含恶意软件」）？

macOS 对 **Electron 官方二进制**（本身也未公证）一视同仁。处理步骤：

1. **恢复被删的 Electron**（被移到废纸篓了就重装依赖）：
   ```bash
   cd ~/lolo/lolo-main
   rm -rf node_modules/electron
   npm install electron@31
   ```
2. **清除它的隔离标记**：
   ```bash
   xattr -cr ~/lolo/lolo-main/node_modules/electron/dist/Electron.app
   ```
3. **临时关闭 Gatekeeper**（一劳永逸，Electron 和其他未签名软件都不再被拦）：
   ```bash
   sudo spctl --master-disable
   ```
   → 系统设置 → 隐私与安全性 → 允许以下来源 → 选「**任何来源**」
4. 再启动：`npm start`（或双击桌面「启动lolo」）
5. 想恢复安全设置：`sudo spctl --master-enable`

> 新版一键脚本已在装完依赖后**自动执行第 2 步**，且桌面启动器**每次启动前也会自动清一次标记**。

### 其他常见问题

| 现象 | 原因 / 解决 |
|---|---|
| `npm install` 卡住很久不动 | 第 3 步的 `export ELECTRON_MIRROR` 没配，或换了新终端窗口。回到第 3 步重配，**在同一窗口**重新 `npm install`。 |
| 提示 Electron 下载失败 / 404 | 同上，确认镜像已设置；或换个网络环境重试。 |
| `cd` 后提示 no such file | 路径不对。用「先输入 `cd ` 再把文件夹拖进终端」的方式填路径最保险。 |
| 启动时报错提到某个依赖缺失 | 回到第 4 步重新执行一次 `npm install`。 |
| 宠物不动 / 不显示动画 | 源码缺 `assets/videos` 里的动画素材，请重新获取一份**完整**的源码压缩包。 |
| 想更新到新版本 | 重新问作者要新版源码 zip（或 `git pull`），然后 `npm install && npm start`。 |

---

## 五、给软件作者的备注（运行时说明）

- 运行时真正在用的只有两个包：**`electron`**（在 devDependencies）和 **`electron-updater`**（自动更新用）。
- `electron-builder` / `javascript-obfuscator` 是**打包 / 混淆**用的，跑源码不需要，但 `npm install` 会一并装上（体积较大）。
- Mac 上必须在**本机**安装 Mac 版 Electron 二进制，**不能**把 Windows 的 `node_modules` 拷过来用（平台不兼容）。
- 动画素材（26 个 APNG）与默认配置均已纳入版本控制，clone 或完整 zip 都不会缺。
