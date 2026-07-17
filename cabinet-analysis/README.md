# 柜机补货 / 领用数据分析台（在线版）

基于单 HTML 页面的柜机补货、领用数据分析工具，支持通过 GitHub 仓库实现跨设备数据同步。

> 注意：本项目与你已有的 `lee-s-warehouse` 仓库相互独立，使用独立的仓库名和 `data.json` 数据文件，避免混淆。

## 在线访问地址

如果已部署到 GitHub Pages，可通过如下链接访问：

```
https://<你的用户名>.github.io/<仓库名>/
```

例如：

```
https://Jasonlee911.github.io/cabinet-data-analysis/
```

## 功能

- **数据导入**：解析「工站-地区补货/领用」透视模板，自动识别常见列名。
- **统计分析**：按大区 / 柜机维度统计补货与领用数量，支持日期范围、大区多选、柜机搜索。
- **聚合 / 详情视图**：默认聚合统计，可切换详情数据。
- **导出 Excel**：导出带标题的双 Sheet 工作簿（补货领用数据 + 统计数据）。
- **柜机-大区映射**：维护柜机归属大区，导入时自动补充新柜机。
- **历史记录**：查看、删除导入批次。
- **GitHub 云同步**：通过 Personal Access Token 把 `mapping` 和 `snapshots` 读写为仓库中的 `data.json`，实现多设备同步。

## 部署到 GitHub Pages

### 方式一：手动上传（推荐，最简单）

1. 在 GitHub 上新建一个仓库，例如 `cabinet-data-analysis`，不要与 `lee-s-warehouse` 同名。
2. 进入仓库 → **Settings → Pages → Build and deployment → Source**，选择 **Deploy from a branch**，分支选 `main`，目录选 `/(root)`，保存。
3. 把本目录下的 `index.html` 和 `README.md` 上传到仓库根目录（通过 GitHub 网页的 **Add file → Upload files**）。
4. 等待几分钟后，访问 `https://<你的用户名>.github.io/<仓库名>/`。

### 方式二：使用 `deploy.ps1` 脚本

1. 生成 GitHub Personal Access Token（见下文）。
2. 在 PowerShell 中进入本目录，运行：

   ```powershell
   .\deploy.ps1
   ```

3. 按提示输入 Token 和希望使用的仓库名（默认 `cabinet-data-analysis`）。脚本会自动创建仓库、上传文件并开启 GitHub Pages。
4. 脚本最后会输出访问链接。

## GitHub Token 说明

1. 登录 GitHub → 右上角头像 → **Settings → Developer settings → Personal access tokens → Tokens (classic)**。
2. 点击 **Generate new token (classic)**。
3. 勾选 **`repo`** 权限（如果是私有仓库则需要完整 `repo`，公开仓库至少 `public_repo`）。
4. 生成后复制 Token（形如 `ghp_xxxxxxxxxxxx`）。
5. **不要将 Token 写入代码或 README 中**。在本应用里，Token 只保存在当前浏览器的 `localStorage` 中。

## 数据同步使用说明

1. 打开在线页面后，点击左侧 **GitHub 同步**。
2. 填写：
   - **GitHub 仓库**：`用户名/仓库名`，例如 `Jasonlee911/cabinet-data-analysis`。
   - **Personal Access Token**：刚才生成的 `ghp_...`。
   - **数据文件路径**：默认 `data.json`。
   - **分支**：默认 `main`。
3. 点击 **测试 Token** 验证连接。
4. 点击 **上传到 GitHub**：把当前浏览器中的 mapping 和 snapshots 保存到仓库的 `data.json`。
5. 在另一台设备打开页面后，点击 **从 GitHub 拉取**：即可把 `data.json` 下载到本地浏览器。

## 数据文件结构

云端 `data.json` 结构如下：

```json
{
  "mapping": [
    { "station": "示例转运中心", "region": "华东区" }
  ],
  "snapshots": [
    {
      "id": "id...",
      "type": "replenish",
      "fileName": "补货数据.xlsx",
      "orderDate": "2026-07-17",
      "importTime": "2026-07-17T09:00:00.000Z",
      "records": [ { "station": "...", "qty": 100, "orderDate": "2026-07-17" } ],
      "stationCount": 1,
      "totalQty": 100
    }
  ]
}
```

## 安全提示

- GitHub Pages 页面本身是公开的，但 Token 只保存在访问者自己的浏览器中，不会出现在仓库代码里。
- 如果你希望数据仓库不公开，可将其设为 **Private**。私有仓库的 GitHub Pages 在免费账号下也是可访问的，但建议不要暴露仓库名给无关人员。
- 定期在 **历史记录** 页面导出备份，以防误操作覆盖云端数据。
