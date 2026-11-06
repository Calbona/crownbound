# 场景树与节点契约

> 版本：v1.0
>
> 整理日期：2026-11-6
>
> 本文档负责：

---

## 1 场景与脚本目录

```
res://scenes/
├── Main.tscn
├──

res://scripts/
├── app/Main.cs
├──
```

---

## 2 `Main.tscn`

```
Main (Node) [Main.cs]
├──
```

---

## 3 路由与层级

### 3.1 屏幕路由

```
```

### 3.2 信号

| 信号 | 处理 |
|---|---|

### 3.3 模态与层级

| 层 | 节点 | `mouse_filter` | 说明 |
|---|---|---|---|

---

## 4 自动加载

加载顺序即依赖顺序：

```
SettingsStore → LocalStatsStore → AudioManager → LanDiscoveryService → LeaderboardService → MatchmakingService
```

自动加载只放跨场景状态与服务，不放 UI。

### 4.1 `SettingsStore`

读写 `user://settings.cfg`：

| 键 | 类型 | 默认 | 说明 |
|---|---|---|---|

### 4.2 `LocalStatsStore`

存储 `user://local_stats.json`：

```json
```

### 4.3 `AudioManager`

### 4.4 `LeaderboardService`

### 4.5

---

## 5 主菜单

```
MainMenu (Control) [MainMenu.cs]
├── Background (ColorRect)
├── Header (VBoxContainer, top-center)
│   ├── TitleLabel (Label)
│   └── SubtitleLabel (Label)
├──
```

`MatchTile` 状态：

| 条件 | 角标 | 可点 |
|---|---|---|
| 教程未完成 | `lock.png` | 否 |
| 完成、未点击过 | `something_new.png` | 是 |
| 完成、已点击过 | 无 |  是 |

### 5.1 `MenuTile.tscn`

```
MenuTile (Button) [MenuTile.cs]
├── Text (VBoxContainer)
│   ├── Title (Label)
│   └── Subtitle (Label, optional)
├── Badge (TextureRect)
```

### 5.2 `IconButton.tscn`

```
IconButton (Button) [IconButton.cs]
├── Icon (TextureRect)
└── Label (Label, optional)
```

- 用 `TextureRect` 而不是 `Button.icon`，便于单独控制过滤与着色。
- 用于结算（再来一局/分享/回主菜单）和对话框（刷新/关闭）。
- `_Ready` 播放点击音；`disabled` 不播。

### 5.3 光标

## 6. 对局屏幕

```
GameScreen (Control) [GameScreen.cs]
├── Background (ColorRect)
├──
```

### 6.1 `Hud.tscn`

```
```

### 6.2 `ResultPanel.tscn`

```
```

---

## 7 教程关卡索引

```
```

---

## 8 匹配屏幕

```
MatchmakingScreen (Control) [MatchmakingScreen.cs]
├── Background (ColorRect)
├──
```

---

## 9 对话框

### 9.1 通用约定

### 9.2 `ConfirmDialog`

```
ConfirmDialog (Control)
├──
```

### 9.3 `SettingsDialog`

```
SettingsDialog (Control)
├── Dimmer
└── Card > VBoxContainer
    ├── Header (HBox: Title, CloseButton)
    ├──
    ├── AccountSection (VBoxContainer)
    │   ├── SectionTitle (Label)
    │   ├── AccountStatus (Label)
    │   ├── NameRow (Label + NameEdit)
    │   └── AccountHint (Label)
    └── Footer (HBox)
        ├── ResetButton (Button)
        ├── SaveButton (TextureButton + Label)
        └── CloseButton (Button)
```

### 9.4 `InfoDialog`

```
InfoDialog (Control)
├── Dimmer
└── Card > VBoxContainer
    ├── Header (HBox: Title, CloseButton)
    ├── TabContainer
    │   ├── Rules (ScrollContainer > RichTextLabel)
    │   ├── Controls (RichTextLabel)
    │   └── About (RichTextLabel)
    └── Footer (HBox: ReplayTutorialButton, CloseButton)
```

- 规则页：胜负、和棋、五种走法、禁区/目标点。
- 操作页：左键选择/落子、右键/Esc 取消、悔棋（本地）、合法落点。
- 关于页：版本、规则版本、字体/音效许可、在线服务与隐私说明。

### 9.5 `LeaderboardPanel`

```
```

### 9.6 `ShareDialog`

```
ShareDialog (Control)
├──
```

### 9.7 `BusyOverlay`

```
BusyOverlay (Control)
├── Dimmer (ColorRect)
├── Spinner (TextureRect)
└── Message (Label)
```

- 只用于加载：切屏、排行榜、匹配、初始化。
- 网络请求超过 255ms 才显示，避免闪烁。
- 不处理取消，取消由 `MatchmakingScreen` 负责。

### 9.8 `Toast`

```
Toast (PanelContainer) > Label
```

- `ToastHost.Show(text, seconds = toast_show_seconds)`，同一时刻一条，新消息替换并重置计时。
- 位置顶部居中。
- 用于非法操作、复制结果、保存成功、网络错误等。棋局判定结果用结算面板。

### 9.9 `PlayModeDialog`

```
PlayModeDialog (Control)
├──
```

---

## 10 尺寸与视觉

### 10.1 基准

- 1280×720，安全边距 24px。
- 盘心 `(640, 388)`。

### 10.2 图标

| 类别 | 源尺寸 | 显示 | 过滤 |
|---|---|---|---|

### 10.3 文本

### 10.4 颜色

---

## 11 资产映射与数据

### 11.1 资产映射

| 资产 | 节点 |
|---|---|

### 11.2 本地数据

| 文件 | 内容 | 写入时机 |
|---|---|---|
| `user://settings.cfg` | | 保存按钮 |
| `user://local_stats.json` | 本机数据 | 对局结束 |

### 11.3 分享的文本

```
```

## 12. 应用壳验收

- [ ] 启动进入主菜单，内容可见。
- [ ] 指针为 `pointer.png`，点击无偏移。
- [ ] 教程未完成时 Match 锁定；完成后解锁并显示 `something_new`，点击后角标消失。
- [ ]

---

> 若后续有修改，以最新版本 scenes-and-nodes 为准。
