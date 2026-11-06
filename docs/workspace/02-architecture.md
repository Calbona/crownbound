# 架构

> 版本：v1.0
>
> 整理日期：2026-11-6
>
> 本文档负责：架构设计

---

## 1 分层

```
```

- `Crown.Core` 不引用任何项目。
- `Crown.Presentation` 引用 `Crown.Core`，不引用 Godot。
- `Crown.Ai` 引用 `Crown.Core`，不引用 Godot。
- Godot 项目引用以上三个项目。
- 局域网 Host 在 Godot 进程内复用 `Crown.Core`；未来的公网服务端也引用同一 `Crown.Core`，不引用 Godot。
- 测试项目不引用 Godot，因此 `dotnet test` 不需要 Godot SDK。

## 2 解决方案

```
crownbound/
├── crownbound.csproj
├── Crown.sln
├── Crown.Core/
├── Crown.Ai/
├── Crown.Presentation/
├── Crown.Tests/
├── scenes/
├── scripts/
├── assets/
└── docs/
```

- `Crown.sln` 不含 Godot 项目，保证 CI 与本地纯 C# 测试不依赖编辑器。
- 所有项目的 TFM 必须一致，以 Godot 4.7 .NET SDK 生成的值为准。

## 3 局面所有权

## 4 数据流

## 5 线程与生命周期

## 6 配置

| 配置 | 位置 | 用途 |
|---|---|---|
| 用户设置 | `user://settings.cfg` | 音量、显示名、AI 难度、`ai/engine_id` 等 |
| 构建配置 | `project.godot` | 设计分辨率、拉伸模式、默认后端地址 |
| 公网地址 | `network/backend_url` | 为空表示未配置公网 |
| 版本 | 常量 | `version`，联机握手校验 |

##  约束清单

- 不允许 UI 复制一份走法生成逻辑。
- 不允许 `Crown.Core` 引用 `Godot`、`System.Net`、`Crown.Ai`。
- 不允许 AI 引擎决定合法性；引擎返回的走法仍要 `TryApply`。
- 不允许在联机中使用本地统计冒充当局结果。
- 不允许把公网未配置当作空排行榜展示。

---

> 若后续有修改，以最新版本 architecture 为准。
