# 高达基地 · 我的 3D 小家 🤖

一个以**高达**为原型的 3D 交互个人主页：RX-78 风格的高达站在一栋剖面小屋旁守护，屋子外观参考"白墙+木饰面+黑框玻璃+屋顶花园"的现代住宅，结构是**剖面娃娃屋**——每个房间代表一个 base 地，房间里有对应城市的元素。

## 怎么运行

和上一个 Demo 一样，需要本地服务器（直接双击会跨域报错）：

```bash
cd my-3d-web
python3 -m http.server 8137
# 打开 http://localhost:8137/gundam-home/
```

## 怎么"更新我在哪个 base"（核心功能）

房间 ↔ 城市对照：

| 房间 | 城市 | 房间元素 |
|------|------|----------|
| 一层西 | 北京 | 故宫红墙、天坛模型、红灯笼 |
| 一层东 | 上海 | 东方明珠、石库门门头、咖啡工作台 |
| 二层西 | 杭州 | 西湖断桥、柳树、雷峰塔、龙井茶桌 |
| 二层东 | 深圳 | 平安大厦模型、双屏工作台、霓虹灯带 |

更新"我现在在哪"的方式（任选）：

1. **改代码（推荐，正式更新）**：打开 `index.html`，最上面找到
   ```js
   const MY_CURRENT_BASE = "beijing";   // ★ 改这一行
   ```
   换成 `shanghai` / `hangzhou` / `shenzhen` 即可。页面上当前基地房间会有**彩色光柱 + 地面光环**标记。
2. **链接参数（临时分享）**：`http://localhost:8137/gundam-home/?base=shenzhen`
3. **页面上点**：点房间或底部城市按钮，实时切换（刷新后回到配置值）。

## 交互一览

- 左键拖拽旋转 / 滚轮缩放 / 右键平移
- 点击任意房间 → 相机飞过去 + 显示城市卡片
- 点击高达 → 它会跳一下（还会转头"看"镜头）
- 🏠 全景按钮 → 回到全局视角

## 三层扩展路线（按需迭代）

1. **换真模型**：把程序化的盒子和高达换成 `.glb` 模型（Sketchfab 有大量免费高达 GLB，poly.pizza 有免费家具），用官方示例 `webgl_loader_gltf` 的写法加载，几行代码。
2. **加房间**：在 `CITIES` 里加城市 + 在对应楼层 `put(...)` 一组小模型即可，结构都参数化了。
3. **做成个人主页**：房间卡片里加你的社交链接；用 `?base=` 参数做"位置时间线"；部署到 Vercel/GitHub Pages（纯静态，零构建）。

## three.js 官方示例对照（GitHub 仓库 examples/ 目录，共 600+ 个）

| 本项目用到的技术 | 对应官方示例 |
|----------------|--------------|
| 剖面小屋 / 场景动画氛围 | `webgl_animation_keyframes`（著名的 LittlestTokyo 等距小屋，风格最接近） |
| 点击房间（射线拾取） | `webgl_interactive_cubes`、`webgl_instancing_raycast` |
| 相机飞行 / 控制 | `misc_controls_orbit` |
| 室内暖光点光源 | `webgl_lights_spotlight`、`webgl_lights_physical` |
| 阴影 | `webgl_shadowmap` 系列 |
| 以后加载高达 GLB | `webgl_loader_gltf` |
