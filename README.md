# mini-crm-app-html

基于 **Vite 8 + xq 插件**的 HTML 产品原型工程（由 `xq-template` 生成）。

所有源码统一放在 `src/` 下；`npm run build` 的构建产物输出到 `src` 同级的 `html/` 目录。

## 快速开始

```bash
npm install
npm run dev      # 开发服务器，编辑 src/ 下的 HTML（页面位于 src/pages/...）
npm run build    # 构建所有页面到 html/
npm run pdf      # 合并 html/ 下所有页面，导出 prototype.pdf
```

> 首次使用 `npm run pdf` 前需安装一次浏览器内核：
> ```bash
> npx playwright install chromium
> ```

## 命令一览

| 命令 | 说明 |
| --- | --- |
| `npm run dev` | 启动 Vite 开发服务器 |
| `npm run build` | 构建全部 HTML 页面到 `html/` |
| `npm run banner` / `build:full` | 构建并生成版权信息头（xq-banner） |
| `npm run preview` | 预览构建产物 |
| `npm run pdf` | 将所有页面合并导出为 `prototype.pdf` |

## 目录结构

```
src/index.html                 # 首页（桌面示例）
src/pages/about/index.html     # 关于页
src/pages/contact/index.html   # 联系页（新增页面加子目录即可）
src/pages/login/index.html     # APP 登录页（移动端原型）
src/pages/home/index.html      # APP 主页
src/pages/user/index.html      # APP 用户中心
src/pages/password/index.html  # APP 修改密码
src/partials/header.html       # 桌面公共头部（doctype/head/body + 顶部导航）
src/partials/footer.html       # 桌面公共底部（页脚 / main.ts / 闭合标签）
src/partials/app-header.html   # APP 公共头部：手机壳 + 顶部标题栏（左槽参数化，返回用 history.back()）
src/partials/app-footer.html   # APP 公共底部
src/ts/main.ts                 # 入口脚本（import bootstrap / bootstrap-icons 的 CSS、xq-util、样式）
src/scss/style.scss            # 站点样式（Bootstrap 变量扩展、主题色、APP 手机壳样式）
public/bootstrap/              # 构建期由 bootstrap-classic 插件拷入的 vendor JS（已 gitignore）
export-pdf.mjs                 # Playwright 合并导出 PDF
vite.config.mjs                # xq 插件接线 + Bootstrap JS 注入 + file:// 兼容处理
tsconfig.json                  # TypeScript 配置（src 下为 TS 入口）
```

> 页面内用 `<xq-include file="/partials/header.html">` 复用公共片段：
> 路径以 `/` 开头时相对源码根 `src/`，即 `src/partials/header.html`。
>
> 移动端 APP 页面（login/home/user/password）统一使用 `app-header.html` + `app-footer.html`：
> 顶部标题栏结构完全相同，标题经 `navTitle` 传入；左侧按钮经 `leftHref / leftClass / leftIcon / leftLabel`
> 参数化（子页返回统一传 `leftHref="javascript:history.back()"`，相对返回上一层、不写死目标页；
> 主页头像入口传具体页面路径；登录页传 `navExtra="d-none"` 隐藏标题栏）。

## 新增页面

1. 在 `src/pages/` 下新建子目录与 `index.html`；
2. 在 `vite.config.mjs` 的 `rollupOptions.input` 增加一条入口：
   ```js
   order: resolve(__dirname, 'src/pages/order/index.html'),
   ```
   `vite-plugin-xq-multi-input` 会自动纳入构建；
3. 用 `<xq-include file="/partials/header.html"></xq-include>` 复用公共片段。
