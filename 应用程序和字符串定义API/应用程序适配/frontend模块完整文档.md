# frontend模块完整文档

本文章包括了`frontend`模块的完整文档。如需样例和更多信息，请参考[**使用frontend模块**](3.%20使用frontend模块.md)。

## `FetchDescriptor` 类

### `__init__`

`frontend.FetchDescriptor`(`domain_name`: `str`,`app_name`: `str`, `subsections`: `str`, `lang`: `str`, `debug_mode`: `bool`, `disable_lang`: `bool`)

创建新的`FetchDescriptor`实例。所有的参数是可选的，不需要全部指定。

- 指定`domain_name`，`app_name`，和`subsections`以把这些数值自动添加到功能引用的参数中。
- 指定`lang`以自定义使用的字符串语言。
- 设定`debug_mode`为`True`以调用功能时输出更多用于调试的信息。
- 设定`disable_lang`为`True`以禁用自定义和自动语言检测，并且永远使用字符串中的`default`语言定义。

### `retrieve_entry_or_fallback`

`frontend.FetchDescriptor.retrieve_entry_or_fallback`(`entry_path`: `str`, `fallback_string`: `str`) -> `str`

`frontend.FetchDescriptor.reof`(`entry_path`: `str`, `fallback_string`: `str`) -> `str`

尝试根据提供的`entry_path`获取并返回对应的字符串。如果创建`FetchDescriptor`时指定了`domain_name`，`app_name`，和`subsections`，这些信息会自动被添加到`entry_path`的前面。

如果找不到对应的字符串和路径名称，该函数会返回提供的`fallback_string`。

### `format_entry_or_fallback`

`frontend.FetchDescriptor.format_entry_or_fallback`(`entry_path`: `str`, `fallback_string`: `str`, `*args`, `**kwargs`) -> `str`

`frontend.FetchDescriptor.feof`(`entry_path`: `str`, `fallback_string`: `str`, `*args`, `**kwargs`) -> `str`

尝试根据提供的`entry_path`获取并返回通过提供的format参数（args和kwargs）格式化后的字符串。如果创建`FetchDescriptor`时指定了`domain_name`，`app_name`，和`subsections`，这些信息会自动被添加到`entry_path`的前面。

如果找不到对应的字符串和路径名称，或者格式化获取的字符串时出现了错误，该函数会返回用format参数格式化后的`fallback_string`。

> [!NOTE]
>  请务必确保`fallback_string`能被成功使用`str.format`函数格式化，并且所有的format参数都被`fallback_string`引用，否则调用此函数时会发生错误。

### `entry_exists`

`frontend.FetchDescriptor.entry_exists`(`entry_path`: `str`) -> `bool`

检查对应的路径名称和字符串是否存在。如果存在，返回`True`。否则，返回`False`。

如果创建`FetchDescriptor`时指定了`domain_name`，`app_name`，和`subsections`，这些信息会自动被添加到`entry_path`的前面。

## 设定本地定义文件

关于更多信息，请见[**调用本地主题定义文件**](调用本地主题定义文件.md)。

### `set_local_themedef`

`frontend.set_local_themedef`(`file_content`: `str`, `overlay`: `bool`=`False`) -> `bool`

设定本地定义文件。设定后，当系统全局主题设定没有请求的字符串时frontend会尝试调用本地定义文件里的定义。

当设定成功时，该函数会返回`True`，否则该函数会返回`False`。

- 设定`overlay`为`True`以把当前定义文件叠加到当前数据上。

> [!NOTE]
>  请将**文件内容**传递到这个函数中，不要传递文件的路径名称。

### `set_local_themedefs`

`frontend.set_local_themedefs`(`file_contents`: `list[str]`, `overlay`: `bool`=`False`) -> `bool`

同时设定多个本地定义文件。该函数与以下操作类似：

```py
frontend.set_local_themedef(file_content1, overlay=overlay)
frontend.set_local_themedef(file_content2, overlay=True)
frontend.set_local_themedef(file_content3, overlay=True)
# ...
```

### `unset_local_themedef`

`frontend.unset_local_themedef`()

取消设定本地定义文件。取消设定后frontend将不再尝试调用本地定义文件。

## 设定默认参数值

以下的函数操作会对**在本文件内**所有之后创建的`FetchDescriptor`生效，除非定义`FetchDescriptor`时指定了其他的数值。你可以指定`None`为数值来取消设定默认值。

### `set_domain`

`frontend.set_domain`(`value`: `str | None`)

设定默认的`domain_name`数值。

### `set_appname`

`frontend.set_appname`(`value`: `str | None`)

设定默认的`app_name`数值。

### `set_subsections`

`frontend.set_subsections`(`value`: `str | None`)

设定默认的`global_subsections`数值。

### `set_debugmode`

`frontend.set_debugmode`(`value`: `bool | None`)

设定默认的`debug_mode`数值。

### `set_lang`

`frontend.set_lang`(`value`: `str | None`)

设定默认的`lang`数值。

### `set_disablelang`

`frontend.set_disablelang`(`value`: `bool | None`)

设定默认的`disable_lang`数值。

---

你也可以修改以下全局变量来设定默认值。该定义会对一个模块中的**所有文件**生效，并且没有通过以上函数进行设定（或取消设定）时会被使用。

### `global_domain`, `global_appname`, `global_subsections`, `global_debugmode`, `global_lang`, `global_disablelang`

全局设定默认数值。