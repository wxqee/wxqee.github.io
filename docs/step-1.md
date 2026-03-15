# 第一步：代码框架与发布配置（GitHub Pages + Cloudflare）

目标：
- 本地可运行与联调
- 可构建并发布到 GitHub Pages
- 绑定自定义域名 wangxiaoqiang.cn（Cloudflare 解析）

## 项目结构
- 根目录：主页入口与静态资源（index.html、CNAME、404.html）
- src/：页面脚本与样式源文件
- public/：发布时直接复制的静态资源（备份 CNAME、404）

## 本地运行
- 安装 Node.js（建议 18+）
- 初始化依赖：
  - npm install
- 启动开发服务器：
  - npm run dev
- 生产构建：
  - npm run build
- 本地预览生产包：
  - npm run preview

## GitHub 仓库与 Pages 配置
- 仓库名建议为：wxqee.github.io（用户/组织主页仓库）
- 推送后在仓库 Settings → Pages：
  - Build and deployment → Source：Deploy from a branch
  - Branch：main
  - Folder：/root（用户主页仓库建议直接从根目录发布）
  - Custom domain：wangxiaoqiang.cn
  - Enforce HTTPS：开启
- 自定义域验证（使用 Cloudflare 代理时必须）：
  - GitHub Pages 会给出一个 TXT 验证记录
  - 在 Cloudflare DNS 添加该 TXT 记录（保持 Proxy 关闭或开启均可，关键是 TXT 正确）

## Cloudflare DNS（参考截图已配置）
- CNAME wangxiaoqiang.cn → wxqee.github.io（Proxied）
- CNAME www → wangxiaoqiang.cn（Proxied）
- 可选：CNAME github → wxqee.github.io（内部跳转使用）
- 注意：Pages 自定义域绑定后，CNAME 文件会被用来保持域名一致性

## CNAME 与 404
- CNAME 文件位于仓库根目录（内容为 `wangxiaoqiang.cn`）
- 404.html 位于仓库根目录，增强错误页体验

## 验证清单
- 本地 `npm run dev` 能打开首页与锚点导航
- 推送后 Pages 成功发布，HTTPS 生效
- 自定义域 wangxiaoqiang.cn 与 www 子域均可正常访问
- 404 页面工作正常（访问不存在路径）

## 后续步骤提示
- 第二步将进行风格与信息架构设计（现代、科技感、严肃专业）
- 第三步将填充内容，严格遵守信息脱敏（简历敏感信息不外泄）
- 第四步进行性能、可访问性、SEO 与监控优化
