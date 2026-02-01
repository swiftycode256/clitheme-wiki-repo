# Documentation for frontend module

This article contains the full documentation for the `frontend` module. For more information, please see [Using the frontend module](3.%20Using%20the%20frontend%20module.md).

## `FetchDescriptor` class

### `__init__`

`frontend.FetchDescriptor`(`domain_name`: `str`,`app_name`: `str`, `subsections`: `str`, `lang`: `str`, `debug_mode`: `bool`, `disable_lang`: `bool`)

Create a new instance of `FetchDescriptor`. All arguments are optional.

- Specify `domain_name`, `app_name`, and `subsections` to automatically add them to entry paths
- Specify `lang` to use a custom locale
- Set `debug_mode=True` to output debugging messages
- Set `disable_lang=True` to disable custom and automatic locale search and always use the `default` entry

### `retrieve_entry_or_fallback`

`frontend.FetchDescriptor.retrieve_entry_or_fallback`(`entry_path`: `str`, `fallback_string`: `str`) -> `str`

`frontend.FetchDescriptor.reof`(`entry_path`: `str`, `fallback_string`: `str`) -> `str`

Attempt to retrieve the entry specified in `entry_path`. If `domain_name`, `app_name`, and `subsections` arguments exist, they will be added before the entry path.

If the entry does not exist, it returns `fallback_string`.

### `format_entry_or_fallback`

`frontend.FetchDescriptor.format_entry_or_fallback`(`entry_path`: `str`, `fallback_string`: `str`, `*args`, `**kwargs`) -> `str`

`frontend.FetchDescriptor.feof`(`entry_path`: `str`, `fallback_string`: `str`, `*args`, `**kwargs`) -> `str`

Attempt to retrieve and format the entry using `str.format` based on `entry_path` and formatting arguments (`args` and `kwargs`). If `domain_name`, `app_name`, and `subsections` arguments exist, they will be added before the entry path.

If the entry does not exist or a formatting error occurs, it formats and returns `fallback_string` instead.

> [!NOTE]
> This function does not handle exceptions caused by formatting `fallback_string`.

### `entry_exists`

`frontend.FetchDescriptor.entry_exists`(`entry_path`: `str`) -> `bool`

Check if the entry at the given entry path exists. Returns `True` if exists and `False` if does not exist.

If `domain_name`, `app_name`, and `subsections` arguments exist, they will be added before the entry path.

## Setting a local theme definition file

For more information, please see [Using local definition files](Using%20local%20definition%20files.md).

### `set_local_themedef`

`frontend.set_local_themedef`(`file_content`: `str`, `overlay`: `bool`=`False`) -> `bool`

Sets a local theme definition file. Afterwards, when an entry does not exist in the current theme data, the data in the local file is searched.

Returns `True` on success and `False` on failure.

- Set `overlay=True` to append the current file onto existing local data

> [!NOTE]
> Pass the file content to this function, not the file path.

### `set_local_themedefs`

`frontend.set_local_themedefs`(`file_contents`: `list[str]`, `overlay`: `bool`=`False`) -> `bool`

Set multiple local theme definition files at once.

```py
frontend.set_local_themedefs([file1, file2, file3])
# Equivalent to:
frontend.set_local_themedef(file1)
frontend.set_local_themedef(file2, overlay=True)
frontend.set_local_themedef(file3, overlay=True)
```

### `unset_local_themedef`

`frontend.unset_local_themedef`()

Unset the local theme definition files. After that, frontend will no longer search for local data.

## Set default argument values

The following functions apply to future `FetchDescriptor` instances created **in the current file only**. Specify `None` to unset the default value.

`frontend.` `set_domain`/`set_appname`/`set_subsections`/`set_lang`(`value`: `str | None`)

`frontend.` `set_debugmode`/`set_disablelang`(`value`: `bool | None`)

Set default arguments for `FetchDescriptor` in the current file.

---

The following global variables can also be modified to set default values, which apply to **all files in the current module** and is used when no default value is set using the above functions.

`frontend.` `global_domain`/`global_appname`/`global_subsections`/`global_debugmode`/`global_lang`/`global_disablelang`

Set default arguments for `FetchDescriptor` globally.
