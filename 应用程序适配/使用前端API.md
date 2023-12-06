# 使用clitheme的frontend模块

为了帮助开发者更方便的添加`clitheme`的适配，本项目提供了一个前端API（frontend）模块。这个模块包含了抓取当前主题中字符串的功能；只需要调用一个函数就可以了。除此之外，frontend还包括了其他的功能，比如检查当前的主题有没有适配请求的字符串。我打算将在后续版本中添加更多的功能，敬请期待。

**注意：** 目前frontend模块只支持使用Python 3编写的应用程序；其他语言编写的程序需要自行访问数据结构。如需更多信息，请见**路径名称和数据结构**。

## 新建`FetchDescriptor`类

前端API使用了一个叫`FetchDescriptor`的一个类，并且所有的功能都包含在这个类里面。初始化`FetchDesciptor`时可以提供如开发者，应用名称，和子路径等信息，调用功能时会自动把这些信息添加到提供的路径名称中，帮你减少代码量。

如需创建`FetchDescriptor`，请导入frontend模块，并使用以下代码：

```py
from clitheme import frontend

f = frontend.FetchDescriptor( \
    domain_name="com.example", \
    app_name="example-app", \
    subsections="example-subsection subsection-2" \
    debug_mode=False, \
    disable_lang=False)
```

`FetchDescriptor`支持以下参数：

- `domain_name`，`app_name`，`subsections`：指定开发者名称，应用名称，和子路径；调用功能时会自动添加到路径中
- `lang`：指定并且覆盖`clitheme`检测到的系统语言信息，并且使用自定义语言（如`zh_CN`，`en_US`，`en_US.UTF-8`等）
- `debug_mode`：如果设置为`True`，调用功能时会输出更多信息，用于调试作用
- `disable_lang`：如果设置为`True`，将会禁用自动语言检测，并且调用功能时将会永远使用当前主题定义中的`default`条目。

### 创建后修改参数/设置

你可以在创建`FetchDescriptor`后修改这些参数。只需要修改对应的变量就可以了。比如说：

```py
f.debug_mode=True
f.disable_lang=True
```

## 全局定义参数

除了在`FetchDescriptor`中定义这些选项和参数，你可以对程序中所有的`FetchDescriptor`设置这些参数。以下变量定义会对所有的`FetchDescriptor`生效，除非创建`FetchDescriptor`时明确指定了这些参数。

**请注意：** 这些全局定义只会影响之后新建的`FetchDescriptor`；更改或设定这些全局定义不会影响已经创建的的`FetchDescriptor`。

```py
from clitheme import frontend

f=FetchDescriptor() # 不会使用定义的全局变量

# 全局设定变量
frontend.global_domain="" # 对应domain_name
frontend.global_appname="" # 对应app_name
frontend.global_subsections="" # 对应subsections
frontend.global_debugmode=False # 对应debug_mode
frontend.global_lang="" # 对应lang
frontend.global_disablelang=False # 对应disable_lang

# 会使用定义的全局变量
f2=FetchDescriptor() 
f3=FetchDescriptor()

# 会忽略全局的global_debugmode=False定义，设定debug_f的debug_mode为True
debug_f=FetchDescriptor(debug_mode=True) 
```

## 使用`retrieve_entry_or_fallback`函数

如需获取当前主题定义的某个字符串，请使用`FetchDescriptor`中的`retrieve_entry_or_fallback`函数。调用时需要提供路径名称和默认字符串。如果当前主题设定没有适配该字符串，则函数会返回提供的默认字符串。你可以将这个函数调用包括在一个`print`语句中，以输出返回的结果。

该函数会读取系统上的语言设置（`$LANG`环境变量），并且会优先返回主题定义中对应语言的字符串。该行为可以在创建`FetchDescriptor`时禁用（详见上面）。

你也可以使用更简短的`reof`函数定义以减少代码量。

```py
[...]
# 对应com.example example-app example-subsection subsection-2 example-entry
f.retrieve_entry_or_fallback("example-entry", "Default text goes here")

# 你也可以使用reof函数，输出结果相同
f.reof("example-entry", "Default text goes here")

# 结果和上面相同
f2=frontend.FetchDescriptor()
f2.reof("com.example example-app example-subsection subsection-2 example-entry", "Default text goes here")
```

## 样例

如需关于前端API调用的样例，请参考本仓库中的`clitheme_example.py`文件。