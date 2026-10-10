# 我的第一个 3D 网页（three.js 入门 Demo）

用 [three.js](https://threejs.org)（当前最新版 v0.186.1）搭的一个太空场景：一颗带光环的行星 + 一颗公转的卫星 + 1500 颗星星的星空，可以拖拽旋转、缩放、点击换颜色。

## 怎么运行

`index.html` 用了 ES Module + import map，**直接双击打开会跨域报错**，需要通过一个本地服务器访问（任选其一）：

```bash
# 方式一：Python（macOS 自带），然后浏览器打开 http://localhost:8137
cd my-3d-web
python3 -m http.server 8137

# 方式二：Node
npx serve my-3d-web

# 方式三：VS Code 装个 "Live Server" 插件，右键 index.html → Open with Live Server
```

不需要 npm install，不需要构建工具——three.js 主库和插件都从 CDN（jsdelivr）按需加载。

## 核心概念（对照 index.html 里的注释块看）

| # | 概念 | 一句话解释 |
|---|------|-----------|
| 1 | **Scene 场景** | 3D 世界的容器，所有东西都 add 进去 |
| 2 | **Camera 相机** | 眼睛，`PerspectiveCamera(视角, 宽高比, 近, 远)` |
| 3 | **Renderer 渲染器** | 底层 WebGL，把场景画到 `<canvas>` 上 |
| 4 | **Light 灯光** | 物体默认是黑的；环境光打底 + 平行光当太阳 |
| 5 | **Mesh 物体** | 几何体（形状）+ 材质（外观）的组合 |
| 6 | **Points 点** | 一次性画几万个点，做星空/粒子 |
| 7 | **OrbitControls** | addon 插件，鼠标拖拽/缩放/平移 |
| 8 | **Raycaster 射线拾取** | 鼠标点击命中的是哪个 3D 物体 |
| 9 | **动画循环** | `setAnimationLoop` 每帧更新 + 重绘，用时间增量 `dt` 保证不同帧率速度一致 |
| 10 | **Group 组** | 把多个物体打包一起变换；"转轴"技巧可实现公转 |

## 心智模型

three.js 就是一个"虚拟摄影棚"：

```
Scene（舞台）
 ├── 灯光（不打光 = 一片黑）
 ├── Mesh(几何体 + 材质)（演员）
 └── Camera（机位）──> Renderer（摄影机拍下来投到屏幕）
```

最常见的三行骨架（任何 three.js 项目都是从这三行开始的）：

```js
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(45, w / h, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();
```

## 下一步可以玩什么

- **官方手册**（最好的教程，从搭建到每章一个小例子）：https://threejs.org/manual/en/
- **官方示例源码**：https://threejs.org/examples/ —— 看中哪个效果直接点 "source" 抄
- **加载真实 3D 模型**：用 `GLTFLoader` 加载 `.glb` 文件（blender 导出 / sketchfab 下载）
- **做产品展示页**：换模型 + 环境贴图（`RoomEnvironment`）+ 玻璃/金属材质
- **工程化**：项目大了以后迁移到 `npm create vite@latest` + `npm i three`，代码几乎不用改，只是 CDN 换成 `import * as THREE from 'three'`

## 注意事项

- import map 里主库和 addons **必须是同一个版本号、同一个 CDN**，混用会报错或行为异常
- 想用别的版本：把 URL 里 `0.186.1` 换掉即可
