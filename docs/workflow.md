# 分支与发布流程

## 1. 分支模型

| 分支 | 用途 |
|---|---|
| `master` | 默认分支、发布线；CI 只监听该分支的推送 |
| `dev` | 日常开发与集成分支 |

约定流程：

1. 功能/修复从 `dev` 切出临时分支（如 `feat/xxx`、`fix/xxx`）。
2. 完成后合并回 `dev`，在 `dev` 上联调。
3. 发布时把 `dev` 合并到 `master`，在 `master` 上打 tag。

`master` 应始终与最新发布版本一致，不直接提交日常改动。

## 2. 版本与 tag

- 版本号取 `git describe --tags` 的结果，tag 形如 `3.0.7`。
- CI 在构建时把版本号写入 `package.json`、`src-tauri/tauri.conf.json`、`src-tauri/Cargo.toml`（Cargo 开发态占位为 `0.0.0`）。
- `updater` tag 由发布流程自动维护，CI 通过 `tags-ignore: [updater]` 跳过它。

## 3. CI 流程

工作流：`.github/workflows/package.yml`。

触发条件：

- push 到 `master`
- push 版本 tag（`updater` 除外）

任务关系：

```
build-extension（独立）

change-version
  ├── build-for-macos
  ├── build-for-windows
  ├── build-for-linux
  └── build-for-windows-fix-runtime

tag 时追加：
  release-update        ← 上述四个构建任务
```

- 普通 push 只构建并上传 artifact。
- 推送 tag 时额外创建 GitHub Release、上传安装包与更新描述。
- Linux 构建覆盖 `x86_64`、`i686`、`aarch64`、`armv7` 四个 target。
- 安装包与扩展资产统一使用 `yihe_` / `Yihe` 命名。

发布所需 secrets（见工作流）：

- `TAURI_PRIVATE_KEY`、`TAURI_KEY_PASSWORD`：更新包签名
- `APPLE_SIGNING_IDENTITY`、`APPLE_CERTIFICATE`、`APPLE_CERTIFICATE_PASSWORD`、`APPLE_TEAM_ID`、`APPLE_API_ISSUER`、`APPLE_API_KEY`、`APPLE_ID`、`APPLE_PASSWORD`：macOS 签名与公证
- `GITHUB_TOKEN`：当前仓库的 Release 写入

## 4. 发布步骤

1. 在 `dev` 上确认改动与 `CHANGELOG` 已更新。
2. 合并 `dev` → `master`。
3. 在 `master` 上创建并推送 tag：

   ```bash
   git tag 3.0.8
   git push origin master --tags
   ```

4. 等待 CI 完成，核对 Release 资产（macOS dmg、Windows exe、Linux deb/rpm/AppImage）。
5. 验证更新描述 `update.json` 已生成到 `updater` tag。

## 5. 独立分支现状

本仓库已与上游分离，发布前需要知道以下差异：

- **更新器**：`src-tauri/tauri.conf.json` 的 `updater.endpoints` 已指向本仓库，但 `updater.active` 当前为 `false`。启用前必须生成新的签名密钥，替换 `TAURI_PRIVATE_KEY` / `TAURI_KEY_PASSWORD` 与配置中的 `pubkey`；旧版本客户端无法用新密钥验证更新。
- **下游分发任务**：原 `homebrew`、`winget`、`trigger-docs-update` 三个任务已移除，不再向上游仓库写入。
- **CI 分支范围**：`package.yml` 只监听 `master`，`dev` 的推送不会触发构建。若需要 `dev` 也有 CI，需要扩展 `on.push.branches`。
- **签名材料**：上表中的 secrets 均需替换为本项目自己的密钥与证书。
- **数据目录**：应用标识为 `com.yihe.desktop`，旧版 `com.pot-app.desktop` 下的配置、备份与缓存不会自动迁移。

## 6. 许可与合规

发布资产（安装包、Release 附件）必须附带 `LICENSE` 与 `docs/NOTICE.md`。修改记录与源码提供方式见 [NOTICE.md §3、§5](./NOTICE.md)。
