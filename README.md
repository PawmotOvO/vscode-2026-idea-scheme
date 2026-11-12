# VSCode 2026 Dark — IntelliJ IDEA 配色方案

将最新版 **VS Code 2026 Dark** 代码配色移植到 IntelliJ IDEA（及 PyCharm、WebStorm、GoLand、CLion 等 JetBrains IDE）的 `.icls` 配色方案。

> 方案名：`VSCode 2026 Dark Fixed`，基于 `Darcula` 继承。

## 安装

1. 打开 IDE：`File → Settings → Editor → Color Scheme`（macOS 为 `Preferences`）。
2. 点击齿轮图标 → `Import Scheme...` → 选择本仓库的 `VSCode-2026-Dark.icls`。
3. **关键一步**：导入后并不会自动选中该方案，请在方案下拉列表里手动选择 `VSCode 2026 Dark Fixed` 并点击 `Apply`。

> 常见坑：只做了「导入」但没「选中」，界面看起来「没变化」。导入 ≠ 选中。

## 配色对照表

| 语法元素 | VS Code 2026 颜色 | 十六进制 | IDEA 对应键 |
| --- | --- | --- | --- |
| 关键字（`class`、`return` 等） | 蓝 | `#61AFEF` | `JAVA_KEYWORD` / `DEFAULT_KEYWORD` |
| 方法调用 / 方法声明 | 紫 | `#D2A8FF` | `METHOD_CALL_ATTRIBUTES` 等 |
| 参数（形参 / 实参） | 橙 | `#D19A66` | `PARAMETER_ATTRIBUTES` / `DEFAULT_PARAMETER` |
| 字符串 | 绿 | `#98C379` | `JAVA_STRING` / `DEFAULT_STRING` |
| 数字 | 浅绿 | `#B5CEA8` | `JAVA_NUMBER` / `DEFAULT_NUMBER` |
| 注释 | 灰 | `#8B949E` | `JAVA_*_COMMENT` / `DEFAULT_COMMENT` |
| 类型 / 类名 | 青 | `#4EC9B0` / `#56B6C2` | `CLASS_NAME_ATTRIBUTES` |
| 变量 / 字段 | 灰白 | `#C9D1D9` | `*_VARIABLE_ATTRIBUTES` / `*_FIELD_ATTRIBUTES` |
| 运算符 | 浅灰 | `#D4D4D4` | `JAVA_OPERATION_SIGN` |
| 字符串转义 | 黄 | `#D7BA7D` | `JAVA_VALID_STRING_ESCAPE` |
| 非法转义 | 红 | `#F48771` | `JAVA_INVALID_STRING_ESCAPE` |
| 常量 / 枚举 | 亮蓝 | `#79C0FF` | `DEFAULT_CONSTANT` |

## 实现说明

JetBrains IDE 的 Java 高亮存在**三套键**，它们会遮蔽通用键 `DEFAULT_*`，因此必须显式补齐：

1. **语法键 `JAVA_*`**：覆盖关键字、字符串、数字、注释、运算符等。
2. **语义键 `*_ATTRIBUTES`**：覆盖方法调用、变量、字段、参数、类名等。
3. **通用键 `DEFAULT_*`**：作为兜底，同时照顾其他语言。

几个容易踩的坑：

- Java 的**方法调用**走 `METHOD_CALL_ATTRIBUTES`，而不是 `DEFAULT_FUNCTION_CALL`。只设 `DEFAULT_*` 时方法调用会显示为白色。
- Java 的**参数**走 `PARAMETER_ATTRIBUTES` / `DEFAULT_PARAMETER`。
- 键名以 JetBrains 官方 `colorSchemeTool.py` 及 `JavaHighlightingColors` 为准；早期的 `JAVA_METHOD_CALL` 之类键名并不存在，会被静默忽略。

## 已知差异

- IntelliJ 的 Java 词法分析器不会像 VS Code 那样把 `static` / `final` 与 `public` / `private` 拆成不同 token，因此**无法做到修饰符与关键字异色**（VS Code 里两者色值相同，实际影响很小）。
- 语义着色依赖 IDE 的索引与语言插件，部分键在未安装对应语言插件时不会生效。

## 文件说明

```
.
├── README.md                  # 本文档
└── VSCode-2026-Dark.icls      # 配色方案文件（导入此文件即可）
```
