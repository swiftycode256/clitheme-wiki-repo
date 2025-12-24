# 主题定义文件中的`header`段落

为了确保使用命令行工具输出当前主题信息时能清晰的辨识到主题，请在文件中包括并定义一些基本信息。

## 基本语法

这些基本信息需要在`header`段落内被定义。如需创建`header`段落，请使用`{header_section}`开始段落和`{/header_section}`结束段落。**在所有的定义文件中，这个段落是需要的。**

```plaintext
{header_section}
    <...>
{/header_section}
```

在`header`段落内可以定义以下信息。你仅需要定义`name`条目，但是建议定义以下所有的信息。

- `name`：定义该主题的名称，如**example-app颜文字主题**。
- `description`: 定义该主题的更多详细信息。
- `version`：定义该主题的版本。比如说，如果你添加或修改了其中的内容，你可以通过这个信息来跟踪修改版本，如`1.0`。
- `locales`：定义主题支持哪些语言。如果支持多个语言，请使用空格分开。比如：`en_US zh_CN`或`English Chinese`
- `supported_apps`：定义主题支持的应用程序。如果支持多个应用程序，请使用空格分开。比如：`example-app example-app-2 another-example-app`

> [!NOTE]
>  除了`description`定义以外，定义内容中的任何终端控制符号和不可见字符使用`clitheme show-info`时将会以明文输出（比如`<\x1b>`）。所以，请不要在这些地方使用终端控制符号；它们不会生效。

下面将展示一个`header`段落样例：

```plaintext
{header_section}
    name: example-app颜文字主题
    description: 为example-app制作的一个可爱的颜文字主题
    version: 1.0
    locales: zh_CN en_US
    supported_apps: example-app example-app-2
{/header_section}
```

## 多行文本段落

[多行文本段落](多行文本段落.md)允许你通过多行定义和输入内容。更多信息请见。

你可以在以下定义使用多行文本段落：

- `description`（使用`[description]`和`[/description]`）
- `supported_apps`（使用`[supported_apps]`和`[/supported_apps]`）
- `locales`（使用`[locales]`和`[/locales]`）

在`[supported_apps]`和`[locales]`中，多个项目标注将以换行分开，而不是空格分开。这允许你在项目名称内插入空格。比如：

```plaintext
{header_section}
    # <...>
    [description]
        Some description here
        Another description here
    [/description]
    [locales]
        Simplified Chinese (zh_CN)
        English (en_US)
    [/locales]
    [supported_apps]
        App one
        App two
        App three
    [/supported_apps]
{/header_section}
```

使用`clitheme show-info`列出信息时会这样展示：

```plaintext
<...>
Description:
Some description here
Another description here
Supported locales:
• Simplified Chinese (zh_CN)
• English (en_US)
Supported apps:
• App one
• App two
• App three
```