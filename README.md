# vue3-org-chart

基于 **Vue 3** 的部门/层级组织架构图展示项目，使用 **DOM + CSS** 绘制节点与连线（非 Canvas 图形引擎）。

**在线体验：** [https://coderjs54.github.io/vue3-org-chart/](https://coderjs54.github.io/vue3-org-chart/)

---

## 技术栈

### 核心框架与运行时

| 技术 | 说明 |
|------|------|
| **Vue 3**（`^3.5.x`） | 前端框架；入口 `createApp` 挂载应用；组件普遍使用 **`<script setup>`** 与 **Composition API**（`ref`、`computed`、`watch`、`onMounted` 等）。 |
| **JavaScript（ES Module）** | `package.json` 中 `"type": "module"`，源码以原生 ESM 组织，由 Vite 打包与开发时解析。 |

### 构建与工程化

| 技术 | 说明 |
|------|------|
| **Vite**（`^6.x`） | 开发与生产构建工具；`npm run dev` / `build` / `preview` 均基于 Vite。 |
| **@vitejs/plugin-vue**（`^5.x`） | 启用单文件组件（`.vue`）编译与 HMR。 |
| **部署基路径** | `vite.config.js` 中配置 `base: '/vue3-org-chart/'`，便于托管在 GitHub Pages 等子路径下；本地根路径预览时需与此保持一致或按需修改。 |

### 样式

| 技术 | 说明 |
|------|------|
| **Sass / SCSS**（`^1.85.x`） | 组件内 `<style lang="scss" scoped>`，用于变量、嵌套等样式组织（如 `OrgChart.vue`、`OrgChartBox.vue`）。 |
| **Scoped CSS** | 各 Vue 组件样式作用域隔离，避免全局污染。 |

### 功能相关依赖

| 技术 | 说明 |
|------|------|
| **html2canvas**（`^1.4.x`） | 将组织架构 DOM 克隆后截图为 Canvas，再导出为 **PNG**（`chart.png`），实现「保存/下载图片」能力。 |

### 浏览器 API（无额外 npm 包）

- **FileReader**：读取用户上传的 JSON 文件。
- **Blob / `<a download>`**：下载模板与导出图片。

---

## 项目结构（简要）

```
vue3-org-chart/
├── index.html              # HTML 入口
├── vite.config.js          # Vite 配置（含 base）
├── package.json
├── public/
│   └── template.json       # JSON 数据模板（可下载）
└── src/
    ├── main.js             # 应用入口，挂载 App
    ├── App.vue             # 根组件，持有组织数据并向子组件传递
    └── components/
        ├── OrgChartBox.vue # 工具栏：上传/模板、缩放、适应、保存、横竖布局切换
        ├── OrgChart.vue    # 递归组织架构节点、连线、缩放与导出逻辑（当前主用）
        └── OrgChart1~3.vue / OrgChartNew.vue  # 其他布局或试验性实现
```

---

## 数据约定

组织节点为树形 **JSON**，典型字段与界面展示对应关系如下（可参考 `public/template.json`）：

| 字段 | 含义 |
|------|------|
| `name` | 节点名称 |
| `type` | 类型（如部门、区等） |
| `code` | 编码 |
| `manager` | 负责人 |
| `members` | 成员数量 |
| `children` | 子节点数组（可选，递归） |

---

## 已实现功能

- 放大、缩小（顶层 `zoom` + 居中补偿）
- 自适应容器（`showFull` 按父容器尺寸计算缩放）
- 视图切换：横向 / 纵向布局
- 将当前架构图导出为图片（html2canvas）
- 下载 JSON 模板、上传自定义 JSON 更新数据

---

## 本地开发

```bash
npm install
npm run dev      # 开发（含 --host，便于局域网访问）
npm run build    # 生产构建
npm run preview  # 预览构建产物
```

---

## 备注

- 根组件 `App.vue` 中内联了多套示例数据（如 `orgData`、`orgDataNew`、`orgDataMore`），当前模板将 **`orgDataMore`** 传给 `OrgChartBox`；接入真实数据时可改为接口或单一数据源。
- 若部署在站点根路径而非 `/vue3-org-chart/`，请将 `vite.config.js` 中的 `base` 改为 `'/'` 或相对路径 `'./'`，并重新构建。
