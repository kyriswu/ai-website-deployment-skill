# AI 网站一键上线助手

将符合条件的 AI 生成前端静态应用审计、构建、打包并发布；只有拿到并复验公开 HTTPS 页面 URL 后才报告成功。

## 适用范围

- 最终产物可由 CDN/Nginx 直接提供，包含 `index.html`
- 构建可完全在本地完成，远端不运行 `npm install`、构建命令或服务端代码
- React、Vue、Svelte、Solid、Astro、Angular 等框架均按最终产物和运行时要求判定，而不是框架或目录名
- 允许浏览器直接调用公开 HTTPS API、CDN、BaaS 或图床；发布物中不得携带秘密

## 不适用

需要自建服务端运行时的后端、数据库、SSR/BFF/Server Actions、项目承载的 WebSocket/SSE、后台任务，以及上线必须依赖 Docker/PM2/服务器配置的应用。

## 发布契约

- ZIP 顶层必须是 `artifact-manifest.json`、`deployment-dossier.json`、`site/`
- 部署端点：`POST https://coze-js-api.devtool.uk/deployment`
- 请求体仅为：`{"content":"<HTTPS ZIP URL>"}`
- 成功必须包含实际的 HTTPS 页面 URL，并由执行端复验 HTTP 2xx、`text/html` 与非空正文。
