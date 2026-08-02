# KDE / keyd macOS 快捷键方案

这份配置用 `keyd` 把左右 `Meta` 键作为 macOS 风格的 Command 键使用。

## 配置文件

- `dot_config/keyd/command.conf`：全局 Meta 映射。
- `dot_config/keyd/app.conf`：Ghostty 的应用级例外映射。
- `dot_config/systemd/user/keyd-application-mapper.service`：按当前活动窗口识别应用。

全局映射如下：

| 快捷键 | 实际按键 | 用途 |
| --- | --- | --- |
| `Meta+C` | `Ctrl+Insert` | 复制 |
| `Meta+V` | `Shift+Insert` | 粘贴 |
| `Meta+X` | `Shift+Delete` | 剪切 |
| `Meta+Tab` | `Alt+Tab` | 切换窗口 |

Ghostty 需要接收原生 Meta 快捷键，因此应用映射会把这些组合还原为 Ghostty 的快捷键，例如：

- `Meta+T` -> Ghostty 新建 tab
- `Meta+D` -> Ghostty 分屏
- `Meta+C/V/A/W` -> Ghostty 原生复制、粘贴、全选、关闭

## 安装/恢复

将配置复制到用户配置目录：

```sh
mkdir -p ~/.config/keyd ~/.config/systemd/user
cp dot_config/keyd/command.conf ~/.config/keyd/command.conf
cp dot_config/keyd/app.conf ~/.config/keyd/app.conf
cp dot_config/systemd/user/keyd-application-mapper.service ~/.config/systemd/user/keyd-application-mapper.service
```

确认当前用户可以访问 keyd socket：

```sh
sudo usermod -aG keyd sun
```

执行后必须退出 KDE 会话并重新登录。重新登录后启用服务：

```sh
systemctl --user daemon-reload
systemctl --user enable --now keyd-application-mapper.service
systemctl --user status keyd-application-mapper.service --no-pager
```

如果服务没有运行，先查看日志：

```sh
journalctl --user -u keyd-application-mapper.service -b --no-pager
```

系统级 keyd 服务也必须运行：

```sh
sudo systemctl enable --now keyd
sudo keyd reload
```

## KDE 设置

KDE 的“系统设置 -> 窗口管理 -> KWin -> 快捷键”中，确认“切换窗口 / Walk Through Windows”使用 `Meta+Tab`。keyd 负责将它转换成 KDE 实际使用的 `Alt+Tab`。

## 注意事项

全局配置会把 Meta+C/V/X 转换成 KDE/多数 Linux 应用常用的 Insert 系列快捷键。Ghostty 通过 `app.conf` 保留自己的原生 Meta 快捷键，所以应用映射服务必须运行，并且 Ghostty 的窗口类名必须识别为 `com-mitchellh-ghostty`。
