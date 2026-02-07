---
name: deploy-to-vercel
description: 部署页面到 Vercel。当用户说"部署"、"上线"、"发布"、"deploy"、"部署到vercel"时使用。
argument-hint: [要部署的目录或文件路径]
---

# 部署到 Vercel

帮助用户将项目或静态页面部署到 Vercel。

## 步骤

1. **检查 Vercel CLI**：运行 `vercel --version`

   - 如果未安装，提示用户先运行 `npm i -g vercel` 安装
   - 如果已安装，继续下一步

2. **检查登录状态**：运行 `vercel whoami`

   - 如果未登录，提示用户先运行 `vercel login` 登录，等待用户确认登录成功后继续
   - 如果已登录，继续下一步

3. **确定部署目录**：从 `$ARGUMENTS` 获取目标路径

   - 如果用户提供了文件路径（如 `.html` 文件），使用该文件所在的目录作为部署目录
   - 如果用户提供了目录路径，直接使用该目录
   - 如果用户没有提供路径，询问用户要部署哪个目录或文件

4. **检查入口文件**：检查部署目录下是否存在 `index.html`

   - 如果主页面文件不叫 `index.html`（如 `a-fancy-webpage.html`），提醒用户：建议重命名为 `index.html`，否则访问时需要输入完整文件名。询问用户是否要重命名
   - 如果已有 `index.html`，继续下一步

5. **检查框架类型**：检查部署目录下是否存在 `package.json`

   - 如果存在 `package.json`，读取其中的 `scripts`、`dependencies`、`devDependencies` 了解项目使用的框架和构建命令
   - 如果不存在（纯静态页面），告知用户将以静态站点方式部署

6. **执行部署**：在部署目录下运行 `vercel --prod --yes`

   - 如果是首次部署该项目，Vercel 会自动创建项目并链接
   - 展示部署日志给用户

7. **部署失败时的排查**：如果部署失败或页面无法正常访问，**必须逐步排查，不要放弃**：

   a. **查看构建日志**：运行 `vercel inspect <deployment-url> --logs` 获取完整构建日志，仔细分析错误原因

   b. **框架识别问题**：如果 Vercel 未正确识别框架：
      - 读取 `package.json` 确认框架类型（Next.js、Vite、CRA 等）
      - 在部署目录创建或修改 `vercel.json`，手动指定框架和构建配置：
        ```json
        {
          "framework": "vite",
          "buildCommand": "npm run build",
          "outputDirectory": "dist"
        }
        ```
      - 常见框架对应的 outputDirectory：Vite/Vue → `dist`，CRA → `build`，Next.js → `.next`

   c. **依赖安装失败**：
      - 检查 `package.json` 中的 Node 版本要求（`engines` 字段）
      - 检查是否有 lockfile（`package-lock.json`、`yarn.lock`、`pnpm-lock.yaml`），如果没有，在本地运行 `npm install` 生成后重新部署
      - 检查是否有私有包或需要特殊 registry 配置

   d. **构建命令失败**：
      - 先在本地运行构建命令（如 `npm run build`）确认是否能成功
      - 如果本地也失败，先修复构建错误再重新部署
      - 如果本地成功但 Vercel 失败，检查环境变量差异，使用 `vercel env` 管理

   e. **路由/404 问题**（SPA 应用常见）：
      - 在 `vercel.json` 中添加 rewrites 规则：
        ```json
        {
          "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
        }
        ```

   f. **重新部署**：修复问题后，再次运行 `vercel --prod --yes` 部署，重复排查直到成功

8. **确认成功**：部署完成后，提取并展示：
   - 项目的生产 URL（Production URL）
   - Vercel 控制台链接
   - 告诉用户"部署成功，页面已上线"
