# 太卜排盘 · GitHub 更新仓库

本目录是 `taibu-updates` 仓库的内容，用于托管应用的自动更新文件。

## 仓库结构

- `version.json` — 应用启动时读取的版本信息（需上传到仓库并开启 GitHub Pages）
- `taibu-paipan.apk` — 安装包（上传为 GitHub Release 附件，不放进仓库）

## 首次部署步骤（只需做一次）

### 1. 创建仓库

1. 打开 https://github.com/new
2. 仓库名填 `taibu-updates`，选 **Public**（Pages 需要公开）
3. 不要勾选任何初始化选项（README/.gitignore），点 Create repository

### 2. 上传 version.json 并开启 Pages

**方式一：网页上传（最简单）**

1. 进入新仓库，点 **Add file → Upload files**
2. 把本目录的 `version.json` 拖进去，点 Commit changes
3. 进入 **Settings → Pages**，Source 选 **Deploy from a branch**，Branch 选 `main`，Save
4. 等 1~2 分钟，访问 `https://1048258747.github.io/taibu-updates/version.json` 应返回 JSON

**方式二：git 命令行**

```bash
git clone https://github.com/1048258747/taibu-updates.git
cd taibu-updates
copy 本目录\version.json .
git add version.json
git commit -m "add version.json"
git push
# 然后在 Settings → Pages 开启 Pages（同上）
```

### 3. 上传 APK 为 Release 附件

1. 进入仓库 **Releases → Create a new release**
2. Tag 填 `v1.0.1`，标题填 `太卜排盘 1.0.1`
3. 把本目录的 `taibu-paipan.apk` 拖到 **Attach binaries** 区域
4. 点 **Publish release**

完成后，APK 下载地址为：
`https://github.com/1048258747/taibu-updates/releases/latest/download/taibu-paipan.apk`
（`latest` 会自动指向最新 Release，以后发版无需改这个地址）

## 以后发新版流程

1. 修改代码 → 同步 assets → 运行 `android/build-apk.ps1`
2. 在 `AndroidManifest.xml` 把 `versionCode` 加 1、`versionName` 更新
3. 运行 `android/deploy-update.ps1 -Notes "更新说明"` 重新生成 `version.json`
4. 把新的 `version.json` 上传覆盖到仓库（网页上传或 git push）
5. 创建新 Release（Tag 用新版本号，如 `v1.0.2`），上传新 APK
6. 用户下次启动应用即收到更新提示

## 可选：安装 GitHub CLI 后一键发布

本机安装 [GitHub CLI](https://cli.github.com/) 并执行 `gh auth login` 后，部署脚本支持自动创建 Release：

```powershell
cd android
powershell -ExecutionPolicy Bypass -File deploy-update.ps1 -Notes "更新说明" -Upload
```
