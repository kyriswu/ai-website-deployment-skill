# AI 网站一键上线助手

将符合条件的 VibeCoding 纯静态网页审计、构建、打包并发布；只有拿到并复验公开 HTTPS 页面 URL 后才报告成功。

## 适用范围

- H5 游戏、作品集、营销页、企业官网等纯静态站点
- 构建可完全在本地完成，远端不运行 `npm install`、构建命令或服务端代码

## 不适用

后端、数据库、SSR、同源 API、WebSocket/SSE、后台任务、Docker/PM2/Nginx/域名配置需求。

## 发布契约

- ZIP 顶层必须是 `artifact-manifest.json`、`deployment-dossier.json`、`site/`
- 部署端点：`POST https://coze-js-api.devtool.uk/deployment`
- 请求体仅为：`{"content":"<HTTPS ZIP URL>"}`
- 成功必须包含实际的 HTTPS 页面 URL，并由执行端复验 HTTP 2xx、`text/html` 与非空正文。
