# 芦苇荡巡护模拟器 — Reed Wetland Patrol Simulator

基于 Godot 4 开发的观鸟巡护模拟游戏。玩家在芦苇荡湿地中识别鸟类、观察行为、完成巡护任务。

## 项目介绍

本项目是一款以鸟类观察为主题的模拟器游戏，包含 50 种湿地鸟类数据库、完整的鸟类行为系统（群聚/飞逃/游走/捕食/反击）、取景框系统、积分与称号系统、微生境刷新系统等。

## 技术栈

- **引擎：** Godot 4.x
- **语言：** GDScript
- **识图 AI：** MiniCPM-V 4.6（Ollama / llama.cpp）

## 开发日志

详细开发过程记录见 [`开发日志_按日期整理.md`](开发日志_按日期整理.md)

## PRD

产品需求文档见 [`PRD for birdwatcing game.txt`](PRD%20for%20birdwatcing%20game.txt)

## 部署避坑

> 本章节记录了将游戏部署到 GitHub Pages + Gitee Pages 过程中踩过的坑和通用解决方案。含凭证的详细配置版仅保存在本地 `.trae/skills/` 下（已加入 `.gitignore`，不会推送）。

### 部署策略概要

- 公开仓库仅存放游戏成品 + 开发日志 + PRD 文档，**不含源代码和构建脚本**
- 使用**临时目录 + orphan 分支**隔离部署内容，避免污染本地 main 分支
- GitHub 推送需配合代理（国内无法直连），Gitee 国内直连无需代理
- 推送后需在仓库设置中启用 Pages 服务并指向 `main` 分支

### 部署优先级

| 优先级 | 平台 | 优势 | 劣势 |
|--------|------|------|------|
| 主部署 | **Gitee Pages** | 国内访问快，带宽无限制 | 免费版每次推送后需手动点"部署" |
| 次部署 | **GitHub Pages** | 国际访问，git push 自动更新 | 国内访问需 VPN |
| 备用 | **Netlify** | git push 自动部署，亚洲 CDN | 免费版带宽仅 ~15GB/月，大文件(>10MB)可能部署卡住 |

**Netlify 通过 GitHub 仓库自动部署**：在 Netlify 控制台连接 GitHub 仓库 → 选 `main` 分支 → 构建命令留空 → 发布目录 `/`。之后每次 push 自动部署，无需手动操作。注意免费版 credit 系统限制（300 credits/月，带宽约 15GB），且单文件超 10MB 可能导致部署卡住，建议仅作为备用方案。

### 避坑指南

#### 1. GitHub 连接超时
- **现象**：`Failed to connect to github.com port 443 after 21000ms`
- **原因**：国内无法直连 GitHub
- **修复**：开启 VPN，查系统代理端口后配置 `git config http.proxy http://127.0.0.1:<端口>`
- **注意**：推送到 Gitee 前要 `git config --unset http.proxy` 取消代理

#### 2. Git Commit 身份未知
- **现象**：`Author identity unknown`
- **原因**：临时仓库无 git 用户配置，安全规则禁止用 `--global`
- **修复**：在本地仓库设置 `git config user.name` 和 `user.email`（不加 `--global`）

#### 3. PowerShell 不支持 Heredoc 语法
- **现象**：`Missing file specification after redirection operator`
- **原因**：PowerShell 不支持 bash 的 `<<'EOF'` 语法
- **修复**：用单行 commit message，或用 PowerShell here-string `@" ... "@`

#### 4. Gitee 推送失败 "Not a git repository"
- **现象**：`'gitee' does not appear to be a git repository`
- **原因**：临时仓库缺少 `credential.helper` 配置或未添加 gitee remote
- **修复**：`git config credential.helper manager` + `git remote add gitee <url>`

#### 5. Godot 导出 PCK 膨胀到 400MB+
有 4 个独立原因，需逐个排查：

| 原因 | 现象 | 修复 |
|------|------|------|
| VRAM 纹理压缩 | 131MB WebP → 510MB .ctex | 导出预设取消勾选"for desktop"和"for mobile" |
| 全量打包 | 包含视频/文档/备份 | 改用 `export_filter="exclude"` + 排除过滤器 |
| 输出在项目内 | 下次导出把上次成品打包进去 | 导出到项目目录之外 |
| 直接改 cfg 无效 | Godot 运行时覆盖 | 必须在编辑器导出对话框里设置 |

#### 6. 飞行精灵素材被误删
- **现象**：导出前试运行发现所有鸟类精灵和立绘消失
- **原因**：鸟类素材通过 `.json` 和 `.tres` 动态引用，`rg` 静态搜索搜不到，误判为未引用
- **修复**：**永远不靠静态搜索删文件**，导出前必须试运行验证
- **规则**：动态加载的资源不能用静态搜索判断是否被引用

#### 7. SVG 图标显示为黑块
- **原因**：Godot 4 的 SVG 导入器不支持 `rgba()` 函数格式
- **修复**：fill 颜色改用十六进制格式（如 `#535B6A` 代替 `rgba(83,91,106,0.68)`）

#### 8. Gallery 字体在浏览器导出后乱码
- **原因**：脚本只设了 `add_theme_font_size_override`（字号），没设 `add_theme_font_override("font", ...)`（字体本身）
- **修复**：两个 override 都要设置，缺一不可

#### 9. 大文件推送警告
- **GitHub**：>50MB 警告，硬限制 100MB/文件
- **Gitee**：网页上传限制 86MB，git push 可更大
- **超 100MB 时**：考虑 Git LFS 或拆分补丁包

### 安全规则（必须遵守）

- **不用** `git config --global`（只用本地仓库配置）
- **不用** `-Force` 参数做文件操作（未经用户批准）
- **用** orphan 分支策略部署到公开仓库
- **不推** 源代码/构建脚本/含凭证的配置文件到公开仓库
- **验证** 文件大小后再推送（GitHub 硬限制 100MB）
- **试运行** 游戏后再导出，验证素材完整
- **不靠** 静态搜索删文件（动态引用不可见）
- **检查** `.gitignore` 是否覆盖 `.trae/`、`.vscode/` 等含本地配置的目录

## 许可

[MIT](LICENSE) © Julie Liu