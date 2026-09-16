# telegram-release

Telegram 全客户端官方安装包镜像 · Automated mirror of official Telegram installers (macOS / Desktop / Android).

自动从各**官方源**拉取**未经修改的官方原版**安装包，归档到 GitHub Releases，每 6 小时检查一次，有新版本自动发布。Telegram 官方有多个各自独立、版本号互不相干的客户端，本仓库按 **tag 前缀分轨**：

| 轨(tag 前缀) | 客户端 | 官方源 | 平台 |
|---|---|---|---|
| `mac-*` | Telegram for macOS（Swift，`ru.keepcoder.Telegram`，12.x） | `osx.telegram.org` Sparkle appcast | macOS |
| `desktop-*` | Telegram Desktop（tdesktop，C++/Qt，7.x） | GitHub `telegramdesktop/tdesktop` releases | Windows x64/x86/arm · macOS(Qt) · Linux |
| `android-*` | Telegram for Android（`org.telegram.messenger`，11.x） | `telegram.org/dl/android/apk`（官网直装，非 Play） | Android APK |

> iOS 是 App Store 独占，无法镜像。

## 下载

到 **[Releases](../../releases)** 按前缀找对应平台，或看 **[Latest](../../releases/latest)**（指向最新 macOS 版）。每个 Release 均附 `SHA256SUMS.txt` 校验和。

- **macOS**：`Telegram-<ver>.<build>.dmg`（安装包）/ `Telegram-<ver>.<build>.app.zip`（Sparkle 更新包，解压即 `.app`）
- **Desktop**：`td-setup-win-x64/x86/arm-<ver>.exe`、`td-portable-win-*-<ver>.zip`、`td-setup-mac-<ver>.dmg`、`td-setup-linux-x64-<ver>.tar.xz`
- **Android**：`Telegram-<ver>.apk`

## 说明

- 全部文件**未经任何修改**，逐字节镜像自各自官方源，仅作下载加速 / 归档留档。
- 官方总下载页：<https://telegram.org/dl>
- macOS appcast：<https://osx.telegram.org/updates/versions.xml>
- Desktop 官方：<https://github.com/telegramdesktop/tdesktop/releases>
- Android 官网 APK：<https://telegram.org/dl/android/apk>

## 自动化

`.github/workflows/mirror.yml`（定时每 6 小时 + 手动 `workflow_dispatch`）：
- **mac 轨**：读官方 appcast，对 feed 里未归档的每个 build 下载 DMG + `.app.zip`，单次上限 `max_per_run`（默认 8），其余交后续定时补齐；
- **desktop 轨**：取 tdesktop 最新 GitHub release，转存其全部平台资产；
- **android 轨**：下官网 APK，用 `pyaxmlparser` 解析 versionName 后归档；
- 每次刷新 [`MIRRORED.md`](MIRRORED.md) 清单并提交（兼作保活，防止定时任务被 GitHub 休眠）。
