# 📦 网站运维部署流程

网站更新由运维人员完成 **两个步骤**：
1. 打包并下载最新站点文件  
2. 部署到服务器后，手动通知 Hub 和搜索引擎  

## 1️⃣ 打包并下载文件

工作流：**Build RSS & Package site files (include IndexNow key)**  
文件：`.github/workflows/site-pack-build.yml`

### 操作步骤
1. 打开 GitHub → **Actions** → 选择该工作流 → **Run workflow**（也可在 main 分支有文件更新时自动触发）。  
2. 等工作流运行完成，在 **Artifacts** 里下载：  
   - `site-files-pack.zip` → 整包（推荐，直接解压上传）  
   - `site-files` → 单文件集合（可选）  

### 包含文件
- `rss.xml`  
- `sitemap.xml`  
- `resources.html`  
- `robots.txt`  
- `README.md`（如存在）  
- `9e4e22c8217239645e4f6718bddfdc1c.txt`（IndexNow 验证 key 文件）

## 2️⃣ 部署并通知

### 部署到服务器
1. 将 `site-files-pack.zip` 解压，**覆盖到网站根目录**。  
2. 确认以下文件可以在公网访问：  
   - `https://www.cfiee-edu.com/rss.xml`  
   - `https://www.cfiee-edu.com/sitemap.xml`  
   - `https://www.cfiee-edu.com/robots.txt`  
   - `https://www.cfiee-edu.com/resources.html`  
   - `https://www.cfiee-edu.com/9e4e22c8217239645e4f6718bddfdc1c.txt`  

> ⚠️ 特别注意：IndexNow key TXT 文件必须能正常访问，否则 Bing/IndexNow 不会接受通知。

### 通知 Hub 与搜索引擎

工作流：**Notify Hub + Google + Bing + IndexNow (manual all-in-one)**  
文件：`.github/workflows/notify-all-indexnow.yml`

1. 打开 GitHub → **Actions** → 选择该工作流 → **Run workflow**。  
2. 默认参数保持不变即可（Hub/Google/Bing/IndexNow 都会运行）。  
3. 运行完成后，在日志中确认：  
   - ✅ Hub 通知成功（返回 `204` 即可）  
   - ✅ Google ping（可能 `200/302` 或 Warning，不影响流程）  
   - ✅ Bing ping（必须成功，否则报错）  
   - ✅ IndexNow（需验证 TXT 文件存在，返回 200/202 即成功）

## ✅ 运维人员 checklist
- [ ] 下载并解压 `site-files-pack.zip`  
- [ ] 部署文件到网站根目录  
- [ ] 检查 5 个关键文件能正常访问  
- [ ] 运行 **Notify Hub + Google + Bing + IndexNow** 工作流  
- [ ] 查看日志确认全部通知成功  
