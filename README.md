# telegram-release

Telegram for macOS 官方安装包镜像 · Automated mirror of official Telegram macOS installers.

自动从官方更新源 [`https://osx.telegram.org/updates/versions.xml`](https://osx.telegram.org/updates/versions.xml)（Sparkle appcast）拉取**官方原版**安装包并归档到 GitHub Releases，每 6 小时检查一次，有新版本自动发布。

## 下载

到 **[Releases](../../releases)** 选版本，或直接看 **[Latest](../../releases/latest)**（最新稳定版）。每个 Release 含：

| 文件 | 说明 |
|---|---|
| `Telegram-<ver>.<build>.dmg` | 官方 DMG 安装包（人工安装用） |
| `Telegram-<ver>.<build>.app.zip` | 官方 Sparkle 更新包（解压即 `Telegram.app`） |
| `SHA256SUMS.txt` | 上述文件的 SHA-256 校验和 |

## 说明

- 全部文件**未经任何修改**，逐字节镜像自 Telegram 官方 `osx.telegram.org/updates/`，仅作下载加速 / 归档留档。
- 官方下载页：<https://telegram.org/dl/osx>
- 更新源（appcast）：<https://osx.telegram.org/updates/versions.xml>
- 版本以构建号 `<build>` 判新旧（号大为新）。

## 自动化

`.github/workflows/mirror.yml`：
- 定时（每 6 小时）+ 手动（`workflow_dispatch`）读取官方 appcast；
- 对 feed 里**尚未归档**的每个 build，从官方源下载 DMG + `.app.zip`，算 SHA-256，创建对应 Release；
- 单次最多新建 `max_per_run`（默认 8）个 Release，其余交由后续定时补齐；
- 每次运行刷新 [`MIRRORED.md`](MIRRORED.md) 清单并提交（兼作保活，防止定时任务被 GitHub 休眠）。
