# 狼叔工作台

线上内容库 · 国内海外全矩阵协同工作台。纯前端单文件应用，零依赖，托管 GitHub Pages 即可使用。

## 功能

- **登录**：账号密码门禁，会话 24 小时有效，支持修改密码
- **首页板块导航**：全部看板 / 国内矩阵 / 海外矩阵 / 定制工作流，点击进入
- **六阶段看板**：待办清单 → 选题策划 → 制作中 → 待发布 → 已发布 → 复盘归档，支持拖拽
- **两类任务**
  - 内容任务：平台（多选）、优先级 P0/P1/P2、负责人、DDL、资产链接
  - 定制工作流：对接客户、客户材料、客户要求、完成时间
- **平台分组**
  - 国内矩阵：B站 / 抖音 / 小红书
  - 海外矩阵：出海短剧 / YouTube / Instagram / Facebook / X
- **筛选与搜索**：类型、平台、优先级、负责人，按任务名/客户/材料模糊搜索
- **数据备份**：导出 / 导入 JSON；逾期任务自动预警
- **多端同步**：接 GitHub 私有仓库做数据存储，多台设备自动同步（见下文）

## 多端同步配置

数据存在 GitHub **私有**仓库里（页面是公开的，数据是私有的），每台设备只需要配置一次。

### 1. 建私有数据仓库

GitHub 右上角 `+` → New repository → 名字 `langshu-data` → **选 Private** → 不要勾选初始化文件 → Create。
然后在仓库里手动新建一个 `data.json`，内容填 `{}`，Commit。（也可以跳过这步，第一次同步会自动创建。）

### 2. 建一个只授权这个仓库的 Token

GitHub → 右上角头像 → Settings → 最下方 Developer settings → **Personal access tokens → Fine-grained tokens → Generate new token**：

- Repository access：选 **Only select repositories** → 只勾 `langshu-data`
- Permissions → Repository permissions → **Contents**：选 **Read and write**
- Generate token，复制 `github_pat_...`

> 不要用权限很大的 classic token（`ghp_...` 带 repo 全权限那种），泄露会波及你所有仓库。

### 3. 在页面里填上

登录 → 右上角齿轮 →「多端同步」→ 填仓库名 `你的用户名/langshu-data` 和上面的 token → 保存并同步。

之后：改任务自动上传（1.6 秒防抖）；打开页面、切回窗口时自动拉取，按任务最后修改时间合并，不用手动操作。

**同步规则**：以任务为单位合并，两边各自新增的都会保留；同一条任务取最后修改的那份；删除会生成墓碑记录，不会被其他设备的旧数据复活（墓碑保留 60 天）。

## 默认账号

| 账号 | 密码 | 角色 |
|---|---|---|
| `langshu` | `langshu888` | 管理员 |
| `guest` | `guest888` | 访客 |

> 登录后点右上角齿轮 →「账号与数据」可修改密码。**上线前务必改掉默认密码。**

## 部署到 GitHub Pages（3 步）

1. **建仓库**：GitHub 右上角 `+` → New repository，名字随意（例：`langshu-workspace`），选 Public，不要勾选任何初始化文件，点 Create。
2. **传文件**：进入仓库 → `uploading an existing file` → 把本目录里的 `index.html` 和 `.nojekyll` 一起拖进去 → Commit changes。
3. **开 Pages**：仓库 Settings → 左侧 Pages → Source 选 `Deploy from a branch` → Branch 选 `main`、目录选 `/ (root)` → Save。

等 1~2 分钟，访问地址为：

```
https://<你的GitHub用户名>.github.io/<仓库名>/
```

## 说明

- 这是**静态站点**：账号校验在浏览器端完成，只是访问门禁，**不是真正的安全认证**——懂技术的人可以绕过。要真实权限控制请接后端。
- 任务数据默认存本浏览器（localStorage）；按上文配置「多端同步」后，数据会存到你自己的 GitHub 私有仓库，多设备共享，作者和其他人都看不到。
- Token 只保存在本机浏览器，不会上传到任何服务器。公共电脑上不要开启同步。
- 清空浏览器数据会丢失本地任务，云端数据仍在，重新填一次 token 同步即可拉回。
