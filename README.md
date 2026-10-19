# 芦苇荡巡护模拟器 — Reed Wetland Patrol Simulator

基于 Godot 4 开发的湿地观鸟巡护模拟游戏：在芦苇荡里观察、识别鸟类，记录行为，完成巡护任务。

## 支持范围与体积

当前版本仅支持电脑浏览器；手机端尚未通过内存优化与真机验证，不会自动加载桌面游戏资源。

- 当前只发布 77 种成鸟；不包含尚未启用的幼鸟、亚成鸟、芦苇杆备用立绘和独立鸟叫。
- 全部运行文件合计约 83.55 MB（含引擎、主包、22 个分包、音乐及封面，未计 HTTP 压缩节省）。
- 发布检查按全部运行文件执行 100 MB 预算；资源与校验值见 game_manifest.json。

## 在线试玩

- 主站（Cloudflare）：https://play.wetlandbirdwatching.com/demo.html
- itch.io：https://zheyu-julie.itch.io/reed-bird-watching-simulator
- GitHub Pages：https://zheyu-julie-liu.github.io/Reed-Bird-watching-simulator/demo.html

## 特色

- 77 种湿地鸟类图鉴（含成鸟/亚成鸟/雏鸟状态），全部带科普故事
- 鸟类行为系统：群聚 / 飞逃 / 游走 / 捕食 / 反击
- 微生境刷新：芦苇荡垂直分层（高空巡飞层 / 苇秆层 / 苇丛层 / 水面层），各住各的鸟
- 昼夜与季节系统：不同时段出现的鸟不同
- 巡护员老周的引导与问答：认鸟答题 / 数鸟挑战 / 问老周科普
- 拍照模式：拖拉取景框拍你的「观鸟笔记」

## 操作

鼠标点击 / 触屏点按即可：点鸟看介绍、点头像和巡护员老周聊天、点入口进芦苇荡深层场景。

## 引擎

Godot 4 · GDScript

## 许可

[MIT](LICENSE) © Julie Liu
