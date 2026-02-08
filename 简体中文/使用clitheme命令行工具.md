# 使用`clitheme`命令行工具

CLItheme的命令行界面包含了大部分面向用户的功能，比如设置和修改主题、获取当前主题信息等。

一般情况下，`clitheme`命令应该已经被安装。如果它不可用，你也可以使用`python3 -m clitheme`。

## `apply-theme` - 应用主题

如需应用主题定义文件，请使用`apply-theme`指令，并且提供文件名称：

```plaintext
$ clitheme apply-theme example-theme.ctdef.txt
这些主题定义文件将会通过以下顺序被应用：
        1: example-theme.ctdef.txt
        2: textemojis.ctdef.txt
如果继续，当前的主题数据将会被覆盖。
是否继续操作？[y/n] y
==> 已成功处理文件
已成功应用主题
```

> [!TIP]
> - 你可以指定`--yes`选项以跳过确认提示
> - 你可以同时指定多个文件名称，并且指定的文件会以从左到右的顺序应用

### 主题数据叠加

默认情况下，这个指令会覆盖之前的主题数据。你可以通过`--overlay`选项以叠加定义和数据。新的定义会被添加到当前的数据中，并且已存在的定义会被覆盖。

```plaintext
$ clitheme apply-theme --overlay example-theme.ctdef.txt
```

> [!NOTE]
> 使用此选项时需要确保之前已经设定过主题。

### 保留临时数据目录

`apply-theme`指令完成后会删除存储数据结构的临时目录。如果你想要保留这个临时目录，你可以指定`--preserve-temp`以保留临时目录。在该模式下，该指令会输出临时目录的路径。

```plaintext
$ clitheme apply-theme --preserve-temp --yes example-theme.ctdef.txt 
==> 已成功处理文件
生成的数据可以在"/tmp/clitheme-temp-XXXXXXXX"查看
已成功应用主题
```

## `update-theme` - 重新应用之前的主题定义文件

`update-theme`指令会重新应用在之前`apply-theme`操作中指定的文件名。如果在之前的操作中使用了`--overlay`选项，则会使用当前主题数据中的所有文件名。

请确保之前使用的主题定义文件没有被删除或移动，否则操作会失败。

你可以指定`--yes`选项跳过确认提示，并指定`--preserve-temp`保留临时目录。

```plaintext
$ clitheme update-theme
这些主题定义文件将会通过以下顺序被应用：
        1: /home/user/Documents/example-theme.ctdef.txt
如果继续，当前的主题数据将会被覆盖。
是否继续操作？[y/n] y
==> 已成功处理文件
已成功应用主题
```

## `remove-theme` - 取消应用/删除当前主题

如需取消应用或删除当前的主题数据，请使用`remove-theme`指令。

```plaintext
$ clitheme remove-theme
当前设定的主题：
[1]: Example theme
[2]: 颜文字样例主题
是否移除这些主题？[y/n] y
已成功移除当前主题数据
```

你可以指定`--yes`选项跳过确认提示。

## `show-info` - 获取当前主题信息

如需获取当前的主题信息，请使用`show-info`指令。该指令会显示主题的详细信息，包括名称，版本，支持的语言和应用程序等。

```plaintext
$ clitheme show-info
当前设定的主题：
[1]: Example theme
版本：1.0
支持的语言：
• en_US
• zh_CN
支持的应用程序：
• example-app
• example-app-two
• another-example

[2]: 颜文字样例主题
版本：1.0
支持的语言：
• zh_CN
支持的应用程序：
• clitheme_example
```

你可以指定`--name`选项以仅显示每个主题的名称或`--file-path`以仅显示每个主题的源文件路径。同时指定这两个选项时，名称和路径都会显示。

```plaintext
$ clitheme show-info --name
当前设定的主题：
[1]: Example theme
[2]: 颜文字样例主题
```

```plaintext
$ clitheme show-info --file-path
当前设定的主题：
/home/user/Documents/example-theme.ctdef.txt
/home/user/Documents/textemojis.ctdef.txt
```

```plaintext
$ clitheme show-info --name --file-path
当前设定的主题：
[1]: Example theme
/home/user/Documents/example-theme.ctdef.txt
[2]: 颜文字样例主题
/home/user/Documents/textemojis.ctdef.txt
```

## `generate-data` - 生成数据结构

`generate-data`指令的功能和`apply-theme`指令相似，只是不会应用主题而已。该指令用于调试用途。

```plaintext
$ clitheme generate-data example-theme.ctdef.txt
==> 已成功处理文件
生成的数据可以在"/tmp/clitheme-temp-XXXXXXXX"查看
```

## `repair-theme` - 重新生成主题数据

`repair-theme`指令会使用当前数据中存储的定义文件来重新生成并应用主题数据，达到修复定义信息和数据库的目的。该指令用于调试用途。

```plaintext
$ clitheme repair-theme
==> 已成功处理文件
已成功应用主题
```

## `--version` - 输出版本信息

`--version`指令会输出版本信息：

```plaintext
$ clitheme --version
CLItheme 版本：2.1
```

# 自定义`clitheme`的提示输出

你可以通过[编写字符串定义](./字符串定义和应用程序API/编写定义文件.md)对`clitheme`的提示输出进行修改和自定义。

## 使用源代码内的本地化文件

为了让CLItheme的命令行工具的提示信息适配中文，本项目使用了主题定义文件来实现这个功能。请在仓库源代码中找到这些文件，并且作为模版使用。如需自定义这些提示信息，请在这些模版上添加修改。之后，使用`clitheme apply-theme`应用主题。

请保留字符串中带有花括号的词汇，比如`{content}`，否则自定义字符串将不会生效。

这些模版可以在以下地址中找到：

- https://gitee.com/swiftycode/clitheme/tree/latest-STABLE/src/clitheme/strings
- https://github.com/swiftycode256/clitheme/tree/latest-STABLE/src/clitheme/strings

## 开发者和应用名称

`clitheme`所有字符串定义的[路径名称](./字符串定义和应用程序API/应用程序适配/1.%20路径名称和数据结构.md)将采用以下格式：

- 开发者名称：`swiftycode`
- 应用程序名称：`clitheme`

这意味着所有的路径名称将会以`swiftycode clitheme`开头。
