# Tabby RS

用 Rust 重写的 [Tabby](https://github.com/Eugeny/tabby) 终端，界面与功能对齐官方 Tabby 1.0.238。本仓库发布 Tabby RS 的安装包，下载见 [Releases](https://github.com/tabby-rs/tabby-rs-release/releases)

## 下载

每个版本的 Release 中，文件名里的 `<版本>` 为发布版本号：

| 平台 | 文件 | 安装与运行 |
|---|---|---|
| Windows x86_64 | `tabby-rs-<版本>-x86_64-pc-windows-msvc-setup.exe` | 安装程序，可选只为当前用户安装（`%LOCALAPPDATA%\Programs\Tabby RS`）或为所有用户安装（`Program Files\Tabby RS`，需要管理员权限）；创建开始菜单快捷方式与卸载项，桌面快捷方式可选 |
| Windows x86_64 | `tabby-rs-<版本>-x86_64-pc-windows-msvc.zip` | 便携版：解压后运行 `tabby-rs.exe`，`conpty\` 目录与它放在一起 |
| Linux x86_64（Debian、Ubuntu） | `tabby-rs-<版本>-x86_64-unknown-linux-gnu.deb` | `sudo apt install ./tabby-rs-<版本>-x86_64-unknown-linux-gnu.deb`，命令为 `tabby-rs`，应用菜单中为 Tabby RS |
| Linux x86_64 | `tabby-rs-<版本>-x86_64-unknown-linux-gnu.AppImage` | `chmod +x` 后直接运行，需要 FUSE |
| Linux x86_64 | `tabby-rs-<版本>-x86_64-unknown-linux-gnu.tar.gz` | 解压后运行 `tabby-rs` |
| 全部 | `SHA256SUMS`、`SHA256SUMS.minisig` | 上面各文件的 SHA-256 与它的签名，用于校验，见下文 |

- 不确定选哪个时：Windows 用 `-setup.exe`，Linux 用 `.AppImage`；这两种和便携包（`.zip`、`.tar.gz`）可在程序内自动更新，`.deb` 的更新需要用 apt 安装新包
- Windows 安装程序支持静默安装：`/S`，加 `/allusers` 为所有用户安装，`/D=<目录>`（放在最后）指定目录
- 卸载：Windows 在「设置 → 应用」中卸载 Tabby RS，或运行安装目录中的 `uninstall.exe`；deb 用 `sudo apt remove tabby-rs`
- 暂无 macOS 版本

## 校验

`SHA256SUMS` 列出各文件的 SHA-256（`sha256sum` 格式），`SHA256SUMS.minisig` 是它的 [minisign](https://jedisct1.github.io/minisign/) 签名，公钥为：

```
RWRVmBvU4XmmeF3KQAS3ZYelCjv/hTi1Q7kPZe7rtc8PyO88xFuoyScf
```

先验证签名，再用 `SHA256SUMS` 校验下载的文件：

```bash
# 验证 SHA256SUMS 的签名（与 SHA256SUMS.minisig 放在同一目录）
minisign -Vm SHA256SUMS -P RWRVmBvU4XmmeF3KQAS3ZYelCjv/hTi1Q7kPZe7rtc8PyO88xFuoyScf

# Linux：校验已下载的文件，没有下载的跳过
sha256sum -c --ignore-missing SHA256SUMS
```

Windows 上用 PowerShell 计算文件的 SHA-256，与 `SHA256SUMS` 中同名文件的值比较（不区分大小写）：

```powershell
Get-FileHash .\tabby-rs-<版本>-x86_64-pc-windows-msvc-setup.exe -Algorithm SHA256
```

## 自动更新

程序启动时与每 12 小时检查本仓库的最新 Release，有新版本时标签栏出现更新按钮。更新前程序用内置的公钥验证 `SHA256SUMS.minisig`，通过后才下载更新包并按 `SHA256SUMS` 校验；验证不通过或下载失败时不安装，点击更新打开发布页。「设置 → Application」的 Automatic Updates 关闭自动检查

## 配置

- 配置在 Linux 的 `~/.config/tabby-rs`、Windows 的 `%APPDATA%\tabby-rs`，卸载时保留；程序旁有 `data` 目录时（便携用法）配置放在其中
- 首次启动时导入官方 Tabby 的配置（`~/.config/tabby/config.yaml`、`%APPDATA%\tabby\config.yaml`），官方配置带 Vault 时询问 Vault 口令后转存其中的密码与密钥
- 插件在「设置 → Plugins」的 Available 中从 [插件索引](https://github.com/tabby-rs/tabby-rs-plugins) 安装

## 问题反馈

在本仓库的 [Issues](https://github.com/tabby-rs/tabby-rs-release/issues) 中提交；程序内「Report a problem」打开的就是这里的新 issue 页面，已填好版本、平台与已安装的插件

## 许可

MIT
