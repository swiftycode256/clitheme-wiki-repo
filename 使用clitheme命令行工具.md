# 使用`clitheme`命令行工具

`clitheme`的命令行界面包含了大部分面向用户的功能，比如设置和修改主题、获取当前主题信息等。

## 程序位置和名称

如果你是通过软件包安装的，一般情况下直接在命令行上执行`clitheme`就可以了。你也可以执行`python3 -m clitheme`，如果`clitheme`不可用。

```plaintext
$ clitheme
Usage:
	clitheme apply-theme [themedef-file] [--overlay] [--preserve-temp]
	clitheme get-current-theme-info [--name] [--file-path]
	clitheme unset-current-theme
	clitheme update-theme
	clitheme generate-data [themedef-file] [--overlay]
	clitheme --version
	clitheme --help
```

如果你使用的是最新的开发版本，则需要直接调用仓库中的模块文件。

```plaintext
# 在仓库根目录执行：
$ python3 -m src.clitheme
<...>
```

## `apply-theme` - 应用主题

如需应用主题定义文件，请使用`apply-theme`指令，并且提供文件名称。比如：

```plaintext
$ clitheme apply-theme example-theme.clithemedef.txt
==> Processing files...
Successfully processed files
==> Applying theme...
Theme applied successfully
```

**提示：** 你可以同时指定多个文件名称，以同时应用这些文件的定义。指定的文件会以从左到右的顺序应用，比如`clitheme apply-theme file1 file2`会先应用`file1`然后再应用`file2`，相当于先应用`file1`然后把`file2`中的定义叠加到当前数据上。

### 主题数据叠加

默认情况下，这个指令会覆盖之前的主题数据。你可以通过`--overlay`选项以叠加定义和数据。新的字符串会被添加到当前的数据中，并且已存在的字符串会被覆盖。

```plaintext
$ clitheme apply-theme --overlay example-theme.clithemedef.txt
```

**注意：** 使用此选项时需要确保之前已经设定过主题。

**提示：** 你可以通过此方法叠加多个语言但字符串路径名称相同的的主题定义文件，因为该功能只会覆盖字符串对应的语言。

### 保留临时数据结构目录

`apply-theme`指令完成后默认会删除存储数据结构的临时目录。如果你想要保留这个临时目录，你可以指定`--preserve-temp`以保留临时目录。

在该模式下，该指令会输出临时目录的路径。你可以通过此路径查看被保留的临时目录。

```plaintext
$ clitheme apply-theme --preserve-temp example-theme.clithemedef.txt 
==> Processing files...
Successfully processed files
View at /tmp/clitheme-temp-XXXXXXXX
==> Applying theme...
Theme applied successfully
```

## `unset-current-theme` - 取消应用/删除当前主题

如需取消应用或删除当前的主题数据，请使用`unset-current-theme`指令。删除数据后，支持的应用程序会停止使用自定义的字符串。

```plaintext
$ clitheme unset-current-theme
Successfully removed the current theme data
```

## `update-theme` - 重新应用之前的主题定义文件

`update-theme`指令会重新应用在之前`apply-theme`操作中指定的主题定义文件，方便修改这些文件时重新应用更改（无需重新指定文件路径）。如果在之前的`apply-theme`操作中使用了`--overlay`选项，则会使用之前所有有关的`apply-theme`操作中指定的文件（当前主题定义中用到的）。

使用这个指令时，请确保之前使用的主题定义文件没有被删除或移动，否则操作会失败。

```plaintext
$ clitheme update-theme
==> Processing files...
Successfully processed files
==> Applying theme...
Theme applied successfully
```

## `get-current-theme-info` - 获取当前主题信息

如需获取当前的主题信息，请使用`get-current-theme-info`指令。该指令会输出主题的详细信息，包括名称，版本，支持的语言和应用程序等。

```plaintext
$ clitheme get-current-theme-info
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
$ clitheme get-current-theme-info --name
Currently installed theme(s):
[1]: Example theme
[2]: 颜文字样例主题
```

```plaintext
$ clitheme get-current-theme-info --file-path
Currently installed theme(s):
/home/user/Documents/example-theme.clithemedef.txt
/home/user/Documents/textemojis.clithemedef.txt
```

```plaintext
$ clitheme get-current-theme-info --name --file-path
Currently installed theme(s):
[1]: Example theme
/home/user/Documents/example-theme.clithemedef.txt
[2]: 颜文字样例主题
/home/user/Documents/textemojis.clithemedef.txt
```

## `generate-data` - 生成数据结构

使用`generate-data`指令会在临时目录中生成数据结构。该指令的功能和`apply-theme`指令相似，只是不会应用主题而已。**该指令仅用于调试和开发用途。**

指定`--overlay`选项以生成把主题文件叠加在当前数据上的数据结构，用法和`apply-theme`相同。

你也可以同时指定多个文件，原理和`apply-theme`一样。

```plaintext
$ clitheme generate-data example-theme.clithemedef.txt
==> Processing files...
Successfully processed files
View at /tmp/clitheme-temp-XXXXXXXX
```

## `--version` - 输出版本信息

`--version`指令会输出版本信息，比如：

```plaintext
$ clitheme --version
clitheme version 2.0
```

详细请见[**关于版本的重要信息**](关于版本的重要信息.md)。
