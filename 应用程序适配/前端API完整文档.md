# 前端API（frontend）文档

本文章包括了`frontend`模块的完整文档。如需样例和更多信息，请参考上一个文章**使用前端API**。

## `FetchDescriptor` 类

### `__init__`

`__init__`(`domain_name`: `str`,`app_name`: `str`, `subsections`: `str`, `lang`: `str`, `debug_mode`: `bool`, `disable_lang`: `bool`)

创建新的`FetchDescriptor`实例。

- 指定`domain_name`，`app_name`，`subsections`以把这些数值自动添加到功能引用的参数中。
- 指定`lang`以自定义使用的字符串语言。
- 设定`debug_mode`为`True`以调用功能时输出更多用于调试的信息。
- 设定`disable_lang`为`True`以禁用自定义和自动语言检测，并且永远使用字符串中的`default`语言定义。

### `retrieve_entry_or_fallback`

`retrieve_entry_or_fallback`(`entry_path`: `str`, `fallback_string`: `str`) -> `str`

尝试根据提供的`entry_path`获取并返回对应的字符串。如果创建`FetchDescriptor`时指定了`domain_name`，`app_name`，和`subsections`，这些信息会自动被添加到`entry_path`的前面。

如果找不到对应的字符串和路径名称，返回提供的`fallback_string`。

### `entry_exists`

`entry_exists`(`entry_path`: `str`) -> `bool`

检查对应的路径名称和字符串是否存在。如果存在，返回`True`。否则，返回`False`。

如果创建`FetchDescriptor`时指定了`domain_name`，`app_name`，和`subsections`，这些信息会自动被添加到`entry_path`的前面。

## 全局变量和选项

以下的变量定义会对所有之后创建的`FetchDescriptor`生效，除非定义`FetchDescriptor`时指定了其他的数值。

### `global_domain`

设定默认的`domain_name`数值。

### `global_appname`

设定默认的`app_name`数值。

### `global_subsections`

设定默认的`global_subsections`数值。

### `global_lang`

设定默认的`lang`数值。

### `global_disablelang`

设定默认的`disable_lang`数值。