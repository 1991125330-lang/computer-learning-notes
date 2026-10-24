# 开发工具与环境（Git + VSCode）

这个板块讲每个开发者每天都会用的工具与版本管理：VSCode 编辑器、Git 版本管理、如何把本地代码同步到 GitHub、如何跨电脑延续自己的工作环境。属于实操技能，边用边练最有效。

## 一、开发环境搭建（VSCode + 插件）

VSCode 是免费开源的代码编辑器，配合插件完成「写代码 - 实时预览 - 检查格式化 - 版本管理」的完整闭环。

### 1.1 必装插件

- **Live Server**：在浏览器实时预览 HTML 页面，改代码保存自动刷新（作者：Ritwick Dey，认准这个原版，避免同名山寨）
- **Prettier**：代码格式化插件，统一代码风格（作者：Prettier）
- **ESLint**：代码质量检查插件，找出错误和潜在问题（作者：Microsoft）
- **GitLens**：Git 可视化增强，鼠标悬浮在某一行代码，即可看到谁、什么时候、写了什么提交信息（作者：GitKraken）

### 1.2 安装与搜索要点

- 插件装在 VSCode 左侧扩展商店（四个方块图标）
- 搜索时输入准确插件名 + 认准作者，避免装到同名山寨版
- ❗常见坑：搜索框只搜 `prett`，会出来一堆 Prettier 相关插件，看不到 Live Server。要清空搜索框，输入准确的 `Live Server`
- Live Server 只在打开 HTML 文件时，右键才出现 `Open with Live Server`，或点右下角状态栏 `Go Live`；不打开 HTML 文件就没有右键菜单

### 1.3 VSCode 窗口与工作区

- 一个 VSCode 窗口 = 一个项目文件夹（工作区），窗口里的 Git、插件、设置都只针对这一个文件夹
- 正确打开方式：左上角「文件」→「打开文件夹」→ 选中项目文件夹
- ❗不要直接打开磁盘根目录（如 D:\）当项目，会把整个磁盘（含回收站、系统文件）当代码扫描，刷出上万条更改
- 想同时开多个项目：左上角「文件」→「新建窗口」，开第二个独立窗口打开另一个项目，两个窗口互不干扰

## 二、安装 Git 本体（Windows）

Git 是版本管理的本体，必须先装它。GitLens 只是给 Git 加可视化功能的辅助插件，**不装 Git 本体，GitLens 装了也没用**。

### 2.1 安装步骤

1. 打开官网 **git-scm.com** → 下载 **Git for Windows**
2. 安装时一路点 Next，默认选项不用改
3. 装好后重启 VSCode

### 2.2 验证是否安装成功

打开 VSCode 终端（下方），输入：

```bash
git --version
```

出现版本号（如 `git version 2.x.x`），就代表安装成功。

### 2.3 配置用户名和邮箱（只需一次）

用来标记是谁提交的代码：

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

## 三、Git 核心概念

Git 是版本管理工具；GitHub 是存放远端 Git 仓库的网站。

### 3.1 两个仓库区分

1. **本地仓库**：存在你电脑上，断网可用。所有修改、版本快照先在本地完成提交（commit）
2. **远端仓库**（origin/GitHub）：网上备份仓库，只接收你主动推送的版本，不会自动扫描电脑文件

### 3.2 通俗比喻

- 本地仓库 = 手机相册（随时拍照、回看旧照片）
- GitHub = 云盘（你主动点上传，才把照片备份上去）
- 云盘不会凭空读出你手机里的照片，得手机先存好再传上去

### 3.3 分支（branch）

- **main**（主分支）：线上正式稳定代码，相当于已印好卖出去的成书，一般不放开发中的内容
- **feature**（功能分支）：单独做一个新功能的分支
- **release**（发布分支）：上线前单独拉出来测试、修 bug 的副本，只改 bug 不加新功能

> 新手一人写项目，只需要用 main 分支，暂不用管 release/feature。

### 3.4 发布分支（release branch）通俗解释

- 主线 main：已上线给用户看的稳定正式版，不能随便乱改
- 开发分支 dev：平时写新功能，经常是半成品、有 bug
- release 分支：当 dev 功能做完准备打包上线 → 单独拉出一条 release，只改小 bug、改版本号、写说明，不再加新功能；测试通过就合并进 main 正式发布
- 多人团队/正式上线才用得到，单人练习用 main 即可

## 四、Git 本地仓库：为什么先建本地仓库

Git 的版本记录能力运行在本地电脑；GitHub 只是备份/共享的远端存储空间。

- 本地：随时写、随时回退版本，不需要联网，断网也能用
- GitHub：被动接收上传，不会自动读取本地文件
- 顺序：先在本地 commit 提交保存版本，再 push 上传到 GitHub

### 4.1 关键误区（新手高频踩坑）

- 打开 VSCode 并不会自动初始化仓库，源代码管理面板那个【初始化仓库】只是提示按钮，只有手动点击才会创建仓库
- 本地提交多少次都不会自动同步到 GitHub，**提交 ≠ 上传**
- **不要在磁盘根目录（如 D:\）初始化 git 仓库**：会把整个磁盘（含回收站、系统文件）当代码扫描，出现上万条更改。只应在单独项目文件夹内初始化

### 4.2 误在磁盘根目录建了仓库怎么办（删除 .git）

1. 打开文件资源管理器，顶部「查看」选项卡 → 勾选【隐藏的项目】（显示隐藏文件）
2. 在磁盘根目录找到隐藏文件夹 **.git**，直接删除它
3. 删除后该磁盘就不再是 git 仓库，那些乱七八糟的上万条更改全部消失

## 五、忽略文件 .gitignore

在你的项目文件夹里新建一个名为 `.gitignore` 的文件，用来告诉 Git 哪些文件不用上传。把下面内容粘贴进去：

```gitignore
# 系统文件
Thumbs.db
.DS_Store
# 编辑器配置
.idea
.vscode
```

## 六、Git 远端仓库：绑定与推送

### 6.1 origin 含义

`origin` 是远端仓库的约定别名，代表你绑定的 GitHub 仓库地址。起个别名，后面操作就不用复制一长串链接。

### 6.2 先建远端仓库（GitHub 网页操作）

GitHub 仓库不会自动凭空生成，必须先手动在 GitHub 网页新建：

1. 打开 github.com，登录账号
2. 右上角加号 → New repository（新建仓库）
3. 填仓库名字：**只用英文、数字、横杠 `-`、下划线 `_`，不要用中文**
4. ❗重点：**不要勾选 Add a README file**（勾选后远端有初始 README，本地仓库为空，第一次 push 会冲突）
5. Add .gitignore 选 No .gitignore；Visibility 按需选 Public（公开）或 Private（私有）
6. 点 Create repository 创建
7. 创建成功后会显示仓库地址，形如 `https://github.com/你的名字/仓库名.git`，复制它

### 6.3 两条关键命令拆解

```bash
# 绑定远端地址（仅建立绑定关系，不上传文件）
git remote add origin https://github.com/你的名字/仓库名.git

# 上传本地 main 分支；-u 记住配对，仅第一次使用
git push -u origin main
```

1. `git remote add origin 地址`：把本地仓库和 GitHub 空仓库绑定（只绑定，不上传任何文件）
2. `git push -u origin main`：上传本地 main 分支；`-u` 记住配对，之后直接 `git push`
3. 如果之前绑定错地址，先 `git remote remove origin` 解除旧绑定

### 6.4 区分【提交】与【发布 Branch】

- **提交**（commit，本地存档）：在源代码管理输入框写备注，按 Ctrl+Enter，只存到自己电脑，不发给 GitHub，相当于给代码拍快照，以后可以回退
- **发布 Branch**（推送，上传）：是那个蓝色按钮，必须先把本地提交做完，才能把 main 分支传到云端仓库

## 七、Git 常用命令速查

```bash
# 查看远端绑定地址（确认绑定是否正确）
git remote -v

# 解除旧绑定（绑定错地址时用）
git remote remove origin

# 查看当前状态与是否同步
git status

# 提交（暂存 + 写备注）
git add .
git commit -m "提交备注"

# 推送（第一次 -u，之后直接 git push）
git push -u origin main
git push

# 拉取远端最新代码到本地
git pull

# 新电脑下载整个项目
git clone https://github.com/你的名字/仓库名.git
```

### 如何判断同步成功

执行 `git status`，输出 `Your branch is up to date with origin/main.` → 本地与远端仓库已同步。
在 VSCode 左下角分支处：
- 出现向上箭头 + 数字：有提交还没推上去（待推送）
- 出现向下箭头 + 数字：GitHub 上有更新，本地还没拉下来
- 没有上下箭头：两边版本一致，已同步

## 八、VSCode 图形化 Git 操作

1. 左侧「源代码管理」图标（分叉图标，快捷键 Ctrl+Shift+G）
2. 蓝色按钮【发布 Branch】：图形界面一键绑定远端 + 推送，替代命令行
3. 推送成功后，源代码管理面板出现向上箭头：有待推送的提交；箭头消失即版本同步

> 命令行卡住时，优先尝试 VSCode 图形【发布 Branch】；国内 HTTPS 访问 GitHub 容易网络超时卡住。

### 想暂时不用 Git，怎么关掉源代码管理面板

1. `Ctrl + ,` 打开设置
2. 搜索框输入 `git.enabled`
3. 取消勾选【Git: Enabled】
4. 重启 VSCode，左侧源代码管理图标直接消失

### 常用快捷键速查（VSCode + Git）

| 快捷键 | 作用 |
|-|-|
| `Ctrl + Shift + G` | 打开/收起左侧「源代码管理」（Git）面板 |
| `Ctrl + Enter` | 在源代码管理输入框写备注后，提交（commit） |
| `Ctrl + ,` | 打开 VSCode 设置 |
| `Ctrl + C` | 终止卡住的命令（如 git push 长时间无反应） |
| 右下角状态栏 `Go Live` | Live Server 启动/停止本地预览 |
| 右键 → `Open with Live Server` | 在 HTML 文件上打开实时预览 |

> 提示：快捷键是「技能类」知识，用几次就记住了，不用死记硬背；记不住就到菜单里点按钮，效果一样。

## 九、故障排查：git push 卡住不动

- **现象**：终端执行 git push 后长时间没反应
- **原因**：国内网络访问 GitHub HTTPS 链路超时，VSCode 里又不会弹出密码框，就静默卡住
- **方案1**：按 `Ctrl + C` 终止；改用 VSCode 左下角登录 GitHub 账号，再点蓝色【发布 Branch】图形推送
- **方案2**：GitHub 已不支持账号密码推送，必须用 **Personal access token**：设置 → Developer settings → Personal access tokens 生成；push 时用户名填账号，密码填 token

## 十、VSCode 设置同步（Settings Sync）

重点：和 Git 代码同步是两套完全独立的功能。

### 设置同步（同步编辑器配置）

- 同步内容：VSCode 插件、主题、字体、快捷键、代码片段、编辑器配置
- 不同步：你的项目代码、本地文件
- 跨电脑：登录同一个 GitHub 账号，开启设置同步，即可一键恢复编辑器环境

### Git 仓库同步（同步代码）

- 同步内容：项目代码、文件、版本记录
- 跨电脑：使用 `git clone` 拉取项目代码

### 一句话区分

- 设置同步 = 编辑器配置（管皮肤、插件、快捷键）
- Git push/pull = 项目代码（管代码、版本记录）
- 两个都登录同一个 GitHub 账号，配合使用

## 十一、跨电脑换设备完整工作流

目标：换电脑后，编辑器环境 + 项目代码全部恢复。

1. 旧电脑：代码修改 → commit 提交 → `git push` 上传到 GitHub 仓库
2. 新电脑打开 VSCode，登录同一个 GitHub 账号，开启【设置同步】→ 自动同步插件、主题、快捷键
3. 新电脑终端克隆项目：`git clone https://github.com/你的名字/仓库名.git`
4. 之后两边电脑统一习惯：修改前先 `git pull`，写完 commit 再 `git push`

## 十二、常见踩坑与故障排查汇总

1. **git push 终端长时间卡住不动**
   原因：国内网络访问 GitHub HTTPS 链路超时
   方案：Ctrl+C 终止，改用 VSCode【发布 Branch】图形推送，或使用 Personal access token

2. **仓库名不要用中文**
   GitHub 仓库地址仅支持英文、数字、横杠 `-`、下划线 `_`

3. **创建 GitHub 仓库时不要勾选 Add a README file**
   勾选后远端存在初始 README，本地仓库为空，第一次 push 会冲突报错

4. **区分两个同步，不要混淆**
   设置同步 = 编辑器配置；Git push = 代码版本

5. **不要在磁盘根目录（如 D:\）初始化 git 仓库**
   会把整个磁盘（含回收站、系统文件）全部当代码扫描，出现上万条更改；只应在单独项目文件夹内初始化

6. **GitHub 网页看不到本地提交**
   本地提交只存在电脑里，必须先在 GitHub 网页新建空仓库，再 push 上传，GitHub 上才会出现仓库和代码

7. **GitLens 装了没反应**
   必须先装 Git 本体（git-scm.com），GitLens 才有数据可看

8. **搜索插件搜错名字**
   搜 `prett` 会看不到 Live Server；搜索框要输入准确插件名并认准作者
