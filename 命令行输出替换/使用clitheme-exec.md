# 使用`clitheme-exec`命令行工具

使用`clitheme-exec`执行命令行指令以应用主题定义文件中的输出匹配规则。你可以使用输出调试选项来获得所需的输出信息，更好的编写匹配规则。

## 执行命令

只需要在你想执行的命令的前面添加`clitheme-exec`就可以了，比如`clitheme-exec ls`或`clitheme-exec gcc test.c`。

```plaintext
$ clitheme-exec ls
<...>
$ clitheme-exec gcc test.c
<...>
```

### 使用sudo执行命令

如需使用`sudo`执行命令，请使用`sudo clitheme-exec`加上你想执行的命令，不要使用`clitheme-exec sudo`。

```plaintext
$ sudo clitheme-exec some-command
<...>
```

如果你已经应用了主题，但是依然受到“没有设定主题”的提示，那是因为使用`sudo`时没有保留你的用户文件夹信息。你可以在`/etc/sudoers`文件中添加`Defaults env_keep += "HOME"`，或找到这一行并删除注释符号，以修复这个问题。

## 可以设定的选项

`clitheme-exec`目前支持以下输出调试选项：

- `--debug`：在每一行输出的前面加上标记。这个标记会包含输出是否为stdout或stderr、匹配规则有没有更改输出等信息。
- `--debug-color`：对每一行添加颜色标记：如果该行输出为stdout，则标记黄色；如果该行输出为stderr，则标记红色。
- `--debug-showchars`：用明文显示以下终端控制字符：
    - ASCII Escape（`{{ESC}}`）
    - Carriage换行（`\r`）
    - 换行符号（`\n`）
    - ASCII Backspace符号（`\x08`）
    - ASCII Bell符号（`\x07`）
- `--debug-newlines`：永远用新的一行显示所有输出（用于末尾没有换行的输出内容）

如需设定这些选项，请在**指定命令之前**指定这些选项，比如说：

```plaintext
$ clitheme-exec --debug --debug-showchars gcc something.c
```