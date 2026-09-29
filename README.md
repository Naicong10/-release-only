# DSH Desktop — 发布物仓库

本仓库**只放发布物**，不含源码。DSH Desktop 的源码仓库是私有的。

## 用途

桌面端「设置 → 更新」从这里拉取更新清单与安装包：

| 通道 | 清单地址 |
|------|----------|
| 稳定 | `releases/latest/download/version.json` |
| 预览 | `releases/download/preview/version.json` |

`version.json` 是**已签名**的更新清单，客户端用内置 Ed25519 公钥验签通过后，
才信任其中的安装包 SHA256。因此：

- 清单与安装包必须配对发布，不能只换其中一个
- 签名私钥丢失则整条更新链失效（私钥不在本仓库）

## 发布流程

见源码仓库的 `docs/update-publish-guide.md`。简言之：

1. 构建并签名安装包
2. `node scripts/release-update.mjs --channel stable`
3. `node scripts/sign-update-manifest.mjs verify release-out/version.json`
4. 把 `release-out/` 里的 `DSH-Setup-<version>.exe` 与 `version.json`
   作为附件上传到对应 tag 的 Release

| 通道 | tag |
|------|-----|
| 稳定 | `v<version>`（**不要**勾 prerelease） |
| 预览 | `preview`（固定 tag，覆盖式更新） |

## 注意事项

- 源码不在此仓库，请勿在此提交代码
- 删除本仓库会导致客户端无法检查更新
