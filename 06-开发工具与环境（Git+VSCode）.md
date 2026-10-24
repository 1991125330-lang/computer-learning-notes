# 开发工具与环境（Git + VSCode）

这个板块讲每个开发者每天都会用的工具与版本管理：VSCode 编辑器、Git 版本管理、如何把本地代码同步到 GitHub、如何跨电脑延续自己的工作环境。属于实操技能，边用边练最有效。

## 开发环境搭建（VSCode + 插件）

VSCode 是免费开源的代码编辑器，配合插件完成「写代码 - 实时预览 - 检查格式化 - 版本管理」的完整闭环。

- Live Server：在浏览器实时预览 HTML 页面，改代码保存自动刷新
- Prettier：代码格式化插件，统一代码风格（作者：Prettier）
- ESLint：代码质量检查插件，找出错误和潜在问题（作者：Microsoft）
- GitLens：Git 可视化增强，鼠标悬浮在某一行代码，即可看到谁、什么时候、写了什么提交信息（作者：GitKraken）

### 常见小坑

- 插件装在 VSCode 左侧扩展商店，认准作者，避免装到同名山寨版
- Live Server 只在打开 HTML 文件时出现右键菜单
- GitLens 依赖 Git 本体，不装 Git 装了也没用

## Git 核心概念

Git 是版本管理工具；GitHub 是存放远端 Git 仓库的网站。

### 两个仓库区分

1. 本地仓库：存在你电脑上，断网可用。所有修改、版本快照先在本地完成提交（commit）
2. 远端仓库（origin/GitHub）：网上备份仓库，只接收你主动推送的版本，不会自动扫描电脑文件

### 分支（branch）

- main（主分支）：线上正式稳定代码，一般不放开发中的内容
- feature（功能分支）：单独做一个新功能的分支
- release（发布分支）：上线前单独拉出来测试、修 bug 的副本，只改 bug 不加新功能

> 新手一人写项目，只需要用 main 分支，暂不用管 release/feature。

## Git 本地仓库：为什么先建本地仓库

Git 的版本记录能力运行在本地电脑；GitHub 只是备份/共享的远端存储空间。

- 本地：随时写、随时回退版本，不需要联网
- GitHub：被动接收上传，不会自动读取本地文件
- 顺序：先在本地 commit 提交保存版本，再 push 上传到 GitHub

> 误区：打开 VSCode 并不会自动初始化仓库，只有手动点【初始化仓库】才会创建；本地提交多少次都不会自动同步到 GitHub，提交 ≠ 上传。

## Git 远端仓库：绑定与推送

### origin 含义

`origin` 是远端仓库的约定别名，代表你绑定的 GitHub 仓库地址。

### 两条关键命令拆解

```bash
# 绑定远端地址（仅建立绑定关系，不上传文件）
git remote add origin https://github.com/你的名字/仓库名.git

# 上传本地 main 分支；-u 记住配对，仅第一次使用
git push -u origin main
```

1. `git remote add origin 地址`：把本地仓库和 GitHub 空仓库绑定
2. `git push -u origin main`：上传本地 main 分支；`-u` 记住配对，之后直接 `git push`

## Git 常用命令速查

```bash
# 查看远端绑定地址
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

## VSCode 图形化 Git 操作

1. 左侧「源代码管理」图标（分叉图标，快捷键 Ctrl+Shift+G）
2. 蓝色按钮【发布 Branch】：图形界面一键绑定远端 + 推送，替代命令行
3. 推送成功后，源代码管理面板出现向上箭头：有待推送的提交；箭头消失即版本同步

> 命令行卡住时，优先尝试 VSCode 图形【发布 Branch】；国内 HTTPS 访问 GitHub 容易网络超时卡住。

## VSCode 设置同步（Settings Sync）

重点：和 Git 代码同步是两套完全独立的功能。

### 设置同步（同步编辑器配置）

- 同步内容：VSCode 插件、主题、字体、快捷键、代码片段、编辑器配置
- 不同步：你的项目代码、本地文件
- 跨电脑：登录同一个 GitHub 账号，开启设置同步，即可一键恢复编辑器环境

### Git 仓库同步（同步代码）

- 同步内容：项目代码、文件、版本记录
- 跨电脑：使用 `git clone` 拉取项目代码

## 跨电脑换设备完整工作流

目标：换电脑后，编辑器环境 + 项目代码全部恢复。

1. 旧电脑：代码修改 → commit 提交 → `git push` 上传到 GitHub 仓库
2. 新电脑打开 VSCode，登录同一个 GitHub 账号，开启【设置同步】→ 自动同步插件、主题、快捷键
3. 新电脑终端克隆项目：`git clone https://github.com/你的名字/仓库名.git`
4. 之后两边电脑统一习惯：修改前先 `git pull`，写完 commit 再 `git push`

## 常见踩坑与故障排查

1. git push 终端长时间卡住不动
   原因：国内网络访问 GitHub HTTPS 链路超时
   方案：Ctrl+C 终止，改用 VSCode【发布 Branch】图形推送

2. 仓库名不要用中文
   GitHub 仓库地址仅支持英文、数字、横杠 `-`、下划线 `_`

3. 创建 GitHub 仓库时不要勾选 Add a README file
   勾选后远端存在初始 README，本地仓库为空，第一次 push 会冲突报错

4. 区分两个同步，不要混淆
   设置同步 = 编辑器配置；Git push = 代码版本

5. 不要在磁盘根目录（如 D:\）初始化 git 仓库
   会把整个磁盘（含回收站、系统文件）全部当代码扫描，出现上万条更改；只应在单独项目文件夹内初始化
