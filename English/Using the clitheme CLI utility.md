# Using the `clitheme` command-line utility

CLItheme's command line utility contains most user-oriented features, such as modifying the theme data and getting its current info.

Under most circumstances, the command `clitheme` should be installed. Otherwise, `python3 -m clitheme` can also be used if it is not available.

## `apply-theme` - Applying theme files

To apply theme definition files, use the `apply-theme` command and provide filenames:

```plaintext
$ clitheme apply-theme example-theme.ctdef.txt
The following definition files will be applied in the following order: 
	1: example-theme.ctdef.txt
        2: textemojis.ctdef.txt
The existing theme data will be overwritten if you continue.
Do you want to continue? [y/n] y
==> Successfully processed files
Theme applied successfully
```

> [!TIP]
> - Specify `--yes` to skip the confirmation prompt
> - You can specify multiple filenames at once, and they will be applied in the order of left to right

### Overlay theme data

Normally, the `apply-theme` command overwrites the existing theme data. You can use the `--overlay` option to append new definitions and data. New entries and rules will be added to pre-existing data, and existing entries will be overwritten.

```plaintext
$ clitheme apply-theme --overlay example-theme.ctdef.txt
```

> [!NOTE]
> Ensure that a theme is already set when using this option.

### Preserve the temporary data folder

The temporary folder used to store data entries will be deleted after `apply-theme` finishes. You can use the `--preserve-temp` option to preserve this folder. The folder path is displayed under this mode.

```plaintext
$ clitheme apply-theme --preserve-temp --yes example-theme.ctdef.txt 
==> Successfully processed files
View at /tmp/clitheme-temp-XXXXXXXX
Theme applied successfully
```

## `update-theme` - Re-apply previously specified files

The `update-theme` command re-applies files previously specified in the previous `apply-theme` command. If `--overlay` was previously used, all filenames used in the current theme data is used.

You must ensure that the previously used theme definition files are not deleted or moved, or the operation will fail.

You can use `--yes` to skip the confirmation prompt and `--preserve-temp` to preserve the temp directory.

```plaintext
$ clitheme update-theme
The following definition files will be applied in the following order: 
	1: example-theme.ctdef.txt
        2: textemojis.ctdef.txt
The existing theme data will be overwritten if you continue.
Do you want to continue? [y/n] y
==> Successfully processed files
Theme applied successfully
```

## `remove-theme` - Unset the current theme

To unset and remove the current theme, use the `remove-theme` command.

```plaintext
$ clitheme remove-theme
Currently installed theme(s):
[1]: Example theme
[2]: Text emojis theme
Do you want to remove the theme(s)? [y/n] y
Successfully removed the current theme data
```

You can use `--yes` to skip the confirmation prompt.

## `show-info` - Get current theme info

To get information about the current theme, use the `show-info` command. It displays information like name, version, supported locales and apps.

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

[2]: Text emojis theme
Version: 1.0
Supported locales: 
• en_US
Supported apps: 
• clitheme_example
```

Use the `--name` option to only show the names and the `--file-path` option to only show the file paths. Both information will be shown if both options are specified.

```plaintext
$ clitheme show-info --name
Currently installed theme(s):
[1]: Example theme
[2]: Text emojis theme
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
[2]: Text emojis theme
/home/user/Documents/textemojis.ctdef.txt
```

## `generate-data` - Generate data folder

The `generate-data` command is similar to the `apply-theme` command, but won't actually apply the theme. This command is used for debugging purposes.

```plaintext
$ clitheme generate-data example-theme.ctdef.txt
==> Successfully processed files
View at /tmp/clitheme-temp-XXXXXXXX
```

## `repair-theme` - Re-generate theme data

The `repair-theme` command uses theme definition files stored in the current data to generate and re-apply the current theme, fixing its entries and database. This command is used for debugging purposes.

```plaintext
$ clitheme repair-theme
==> Successfully processed files
Theme applied successfully
```

## `--version` - Get current version

`--version` outputs the current version:

```plaintext
$ clitheme --version
CLItheme version 2.1
```

# Customize the messages of `clitheme`

You can customize the output messages of `clitheme` by [writing string entries](./String%20entries%20and%20frontend%20API/Writing%20definition%20files.md).

## Using localization files in source code

To add internationalization support, this project used theme definition files in its source code. You can find them in the repository and use them as templates. To customize the output messages, you can make changes on top of the templates. Then, use `clitheme apply-theme` to apply it.

Phrases with brackets like `{content}` must be preserved in the content, or the custom string entry won't take effect.

The templates can be found here:

- https://gitee.com/swiftycode/clitheme/tree/latest-STABLE/src/clitheme/strings
- https://github.com/swiftycode256/clitheme/tree/latest-STABLE/src/clitheme/strings

## Domain and app names

The [path names](./String%20entries%20and%20frontend%20API/Developer%20docs/1.%20Path%20names%20and%20data%20structure.md) of string entries used by `clitheme` follows this format:

- Domain name: `swiftycode`
- App name: `clitheme`

This means that all path names will start with `swiftycode clitheme`.

