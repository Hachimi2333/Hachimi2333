# Hachimi2333 website

使用 Vue 3、TypeScript 和 Vite 构建的静态个人网站，使用 shadcn-vue 按钮组件和 Tailwind CSS。Inter 字体随构建产物自托管，中文回退到系统字体。

## 本地开发

需要 Node.js 22.12 或更新版本以及 npm。建议使用受支持的 Node.js LTS 版本。

```sh
cd website
npm ci
npm run dev
```

使用终端输出的地址访问开发页面。`npm run typecheck` 检查类型，`npm run build` 检查类型并生成 `dist/`，`npm run preview` 本地预览构建结果。

## 部署到 Cloudflare Workers

项目通过 `wrangler.jsonc` 将 `dist/` 配置为 Workers Static Assets，无需 Worker 脚本。先登录 Cloudflare 并确认账号拥有部署权限，然后在 `website/` 运行：

```sh
npx wrangler login
npm run deploy
```

部署脚本会先构建再调用 Wrangler。可按需在 `wrangler.jsonc` 中修改 Worker 名称，并在 Cloudflare 控制台配置自定义域名。使用 Workers Builds 连接此仓库时，将根目录设为 `website`，构建命令设为 `npm ci && npm run build`，部署命令设为 `npx wrangler deploy`。

头像通过 `https://static.hachimi2333.top/avatar.webp` 加载。个人简介和链接位于 `src/App.vue`，样式位于 `src/style.css`；shadcn-vue 配置位于 `components.json`，按钮组件代码位于 `src/components/ui/button/`。可使用 `npx shadcn-vue@latest add <组件名>` 按需添加组件。
