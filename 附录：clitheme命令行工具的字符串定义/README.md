# 自定义CLItheme命令行界面的输出

你可以通过编写主题定义文件对CLItheme的命令行界面输出进行修改和自定义。

有关编写主题定义文件的信息，请参考[应用程序和字符串定义API/编写主题定义文件/基本用法](../应用程序和字符串定义API/编写主题定义文件/基本用法.md)。

## 使用源代码内的本地化文件

为了让CLItheme的命令行工具的提示信息适配中文，本项目使用了主题定义文件来实现这个功能。请在仓库源代码中找到这些文件，并且作为模版使用。如需自定义这些提示信息，请复制这些文件，并且添加你想要的修改。之后，请使用`clitheme apply-theme`应用主题。应用后，更改会立即生效。

请保留字符串中带有花括号的词汇，比如`{content}`，否则自定义文本将不会生效。

这些文件可以在以下地址中找到：

- https://gitee.com/swiftycode/clitheme/tree/latest-STABLE/src/clitheme/strings
- https://github.com/swiftycode256/clitheme/tree/latest-STABLE/src/clitheme/strings

## 开发者和应用名称

CLItheme所有字符串定义的路径名称将采用以下开发者和应用名称，并且永远会以这些名称开头。详见[应用程序和字符串定义API/应用程序适配/路径名称和数据结构](../应用程序和字符串定义API/应用程序适配/1.%20路径名称和数据结构.md)。

- 开发者名称：`swiftycode`
- 应用程序名称：CLItheme

所有的路径名称将会以`swiftycode clitheme`开头。比如说：`swiftycode clitheme example-definition`

## 为什么不在文档中提供字符串定义的信息？

自`v2.0`的文档开始，每个字符串定义的详细信息将不再提供。这是因为一个版本中可能会添加很多的提示信息和对应的字符串定义；维护文档中的信息会过于麻烦和耗时。并且，所有需要的信息都可以在源代码中的主题定义文件找到，没必要在文档中重复这些多余的信息。