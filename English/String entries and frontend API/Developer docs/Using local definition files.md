# Using local theme definition files

Your application can also use a local theme definition file. When set, if an entry does not exist in the current theme data, the data in the local file will be searched. Typically, you should place the file in your app's resource files or within its code.

## Setting the file

Use `frontend.set_local_themedef` to set a local theme definition file and pass its file content as arguments. This function returns `True` on success and `False` on failure.

```py
from clitheme import frontend
filename = #...
content=open(filename, "r", encoding="utf-8").read()
frontend.set_local_themedef(content)
```

After setting, it will be effective for retrieval of string entries within all files in the same Python module.

## Setting multiple files

Use `frontend.set_local_themedefs` to set multiple local theme definition files at once and pass a list of file contents as arguments. You can also use `set_local_themedef` with the `overlay=True` argument.

```py
from clitheme import frontend
# ...
frontend.set_local_themedefs([file_content1, file_content2, file_content3])
# Can also be used:
frontend.set_local_themedef(file_content1)
frontend.set_local_themedef(file_content2, overlay=True)
frontend.set_local_themedef(file_content3, overlay=True)
```

## Unset the current file

Use `frontend.unset_local_themedef` to unset the current local file. After that, frontend will no longer try to search for data in the local file(s).

```py
frontend.unset_local_themedef()
```
