# 使用`clitheme`命令行工具

CLItheme的命令行界面包含了大部分面向用户的功能，比如设置和修改主题、获取当前主题信息等。

## 程序位置和名称

如果你是通过软件包安装的，一般情况下直接在命令行上执行`clitheme`就可以了。你也可以执行`python3 -m clitheme`，如果`clitheme`不可用。

```plaintext
$ clitheme
Usage:
        clitheme apply-theme [file] (--overlay) (--preserve-temp) (--yes)
        clitheme show-info (--name) (--file-path)
        clitheme remove-theme
        clitheme update-theme (--yes)
        clitheme generate-data [file] (--overlay)
        clitheme repair-theme
        clitheme --version
        clitheme --help
```

> [!TIP]
> 如果你使用的是最新的开发版本，则需要直接调用仓库中的模块文件。
> 
> ```plaintext
> # 在仓库根目录执行：
> $ python3 -m src.clitheme
> <...>
> ```

## `apply-theme` - 应用主题

如需应用主题定义文件，请使用`apply-theme`指令，并且提供文件名称。比如：

```plaintext
$ clitheme apply-theme example-theme.ctdef.txt
==> Successfully processed files
Theme applied successfully
```

> [!TIP]
> - 你可以指定`--yes`选项以跳过确认提示。
> - 你可以同时指定多个文件名称，以同时应用这些文件的定义。指定的文件会以从左到右的顺序应用，比如`clitheme apply-theme file1 file2`会先应用`file1`然后再应用`file2`，相当于先应用`file1`然后把`file2`中的定义叠加到当前数据上。

### 主题数据叠加

默认情况下，这个指令会覆盖之前的主题数据。你可以通过`--overlay`选项以叠加定义和数据。新的字符串会被添加到当前的数据中，并且已存在的字符串会被覆盖。

```plaintext
$ clitheme apply-theme --overlay example-theme.ctdef.txt
```

> [!NOTE]
>  使用此选项时需要确保之前已经设定过主题。

> [!TIP]
> 你可以通过此方法叠加多个语言但字符串路径名称相同的的主题定义文件，因为该功能只会覆盖字符串对应的语言。

### 保留临时数据结构目录

`apply-theme`指令完成后默认会删除存储数据结构的临时目录。如果你想要保留这个临时目录，你可以指定`--preserve-temp`以保留临时目录。

在该模式下，该指令会输出临时目录的路径。你可以通过此路径查看被保留的临时目录。

```plaintext
$ clitheme apply-theme --preserve-temp example-theme.ctdef.txt 
==> Successfully processed files
View at /tmp/clitheme-temp-XXXXXXXX
Theme applied successfully
```

## `remove-theme` - 取消应用/删除当前主题

如需取消应用或删除当前的主题数据，请使用`remove-theme`指令。删除数据后，支持的应用程序会停止使用自定义的字符串。

```plaintext
$ clitheme remove-theme
Successfully removed the current theme data
```

## `update-theme` - 重新应用之前的主题定义文件

`update-theme`指令会重新应用在之前`apply-theme`操作中指定的主题定义文件，方便修改这些文件时重新应用更改（无需重新指定文件路径）。如果在之前的`apply-theme`操作中使用了`--overlay`选项，则会使用之前所有有关的`apply-theme`操作中指定的文件（当前主题定义中用到的）。

使用这个指令时，请确保之前使用的主题定义文件没有被删除或移动，否则操作会失败。

你可以指定`--yes`选项以跳过确认提示。

```plaintext
$ clitheme update-theme
==> Successfully processed files
Theme applied successfully
```

## `show-info` - 获取当前主题信息

如需获取当前的主题信息，请使用`show-info`指令。该指令会输出主题的详细信息，包括名称，版本，支持的语言和应用程序等。

```plaintext
$ clitheme show-info
Currently installed theme(s):
[1]: Example theme
Version: 1.0
Supported locales: 
• en_US
• zh_CN
Supported apps: 
• example-app
• example-app-two
• another-example

[2]: 颜文字样例主题
Version: 1.0
Supported locales: 
• zh_CN
Supported apps: 
• clitheme_example
```

你可以指定`--name`选项以仅显示每个主题的名称或`--file-path`以仅显示每个主题的源文件路径。同时指定这两个选项时，名称和路径都会显示。

```plaintext
$ clitheme show-info --name
Currently installed theme(s):
[1]: Example theme
[2]: 颜文字样例主题
```

```plaintext
$ clitheme show-info --file-path
Currently installed theme(s):
/home/user/Documents/example-theme.ctdef.txt
/home/user/Documents/textemojis.ctdef.txt
```

```plaintext
$ clitheme show-info --name --file-path
Currently installed theme(s):
[1]: Example theme
/home/user/Documents/example-theme.ctdef.txt
[2]: 颜文字样例主题
/home/user/Documents/textemojis.ctdef.txt
```

## `repair-theme` - 修复/重新生成主题数据

`repair-theme`指令会使用当前数据中存储的定义文件来重新生成并应用主题数据，达到修复定义信息和数据库的目的。**该指令仅用于调试和开发用途。**

```plaintext
$ clitheme repair-theme
==> Successfully processed files
Theme applied successfully
```

## `generate-data` - 生成数据结构

使用`generate-data`指令会在临时目录中生成数据结构。该指令的功能和`apply-theme`指令相似，只是不会应用主题而已。**该指令仅用于调试和开发用途。**

指定`--overlay`选项以生成把主题文件叠加在当前数据上的数据结构，用法和`apply-theme`相同。

你也可以同时指定多个文件，原理和`apply-theme`一样。

```plaintext
$ clitheme generate-data example-theme.ctdef.txt
==> Successfully processed files
View at /tmp/clitheme-temp-XXXXXXXX
```

## `--version` - 输出版本信息

`--version`指令会输出版本信息，比如：

```plaintext
$ clitheme --version
clitheme version 2.0
```

# 自定义命令行界面的输出

你可以通过[编写主题定义文件](./应用程序和字符串定义API/编写定义文件.md)对CLItheme的命令行界面输出进行修改和自定义。

## 使用源代码内的本地化文件

为了让CLItheme的命令行工具的提示信息适配中文，本项目使用了主题定义文件来实现这个功能。请在仓库源代码中找到这些文件，并且作为模版使用。如需自定义这些提示信息，请复制这些文件并添加你想要的修改。之后，请使用`clitheme apply-theme`应用主题。应用后，更改会立即生效。

请保留字符串中带有花括号的词汇，比如`{content}`，否则自定义文本将不会生效。

这些文件可以在以下地址中找到：

- https://gitee.com/swiftycode/clitheme/tree/latest-STABLE/src/clitheme/strings
- https://github.com/swiftycode256/clitheme/tree/latest-STABLE/src/clitheme/strings

## 开发者和应用名称

CLItheme所有字符串定义的[路径名称](./应用程序和字符串定义API/应用程序适配/1.%20路径名称和数据结构.md)将采用以下开发者和应用名称：

- 开发者名称：`swiftycode`
- 应用程序名称：`clitheme`

所有的路径名称将会以`swiftycode clitheme`开头。比如说：`swiftycode clitheme example-definition`
