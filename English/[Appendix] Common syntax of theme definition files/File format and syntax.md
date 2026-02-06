# Overall file format and syntax

This article details the overall file format and section layout of theme definition files.

## File extension

Although there are no strict requirements on file extension, the recommended extension is `.ctdef.txt`. This allows easier identification of files, while the `.txt` extension ensures that the correct application is used (e.g. text editor).

## File encoding

All definition files must use `UTF-8` encoding. This is especially important on Windows, since some editors might use the default `GBK` encoding. If encoding issues were encountered, try converting the file to `UTF-8` encoding.

## Sections and contents

In theme definition files, sections are used to differentiate different types of content. Each section corresponds to a specific feature, containing different content definitions. Some of them include `{entries}`, `{substrules}`, `{manpages}`.

All definition files contain the `header` section (`{header}`), which defines basic information and metadata for the theme, such as name, description, and version. For more information, see the [header section](header%20section.md) article.

> [!NOTE]
> The examples in this documentation may contain indents. It is only used for readability and is not required. However, it is recommended to use indents for readability.

## Comments

Comments allow you add additional information to the file, such as explaining definitions. They start with `#` and are not processed and read.

```plaintext
# this is a comment
```

> [!NOTE]
> Comments can only be defined on new lines. They can't be defined at the end of lines.
> 
> ```plaintext
> # Incorrect usage:
> {header} # Some comments here
> ```

## Specifying minimum supported version

You can specify the minimum supported CLItheme version for the definition file, ensuring newly added features are always available. Add the `!require_version` definition **at the start of the file**:

```plaintext
!require_version 2.1
```

> [!NOTE]
> This must be defined before any other lines except comments.

You can specify versions `2.0` or greater in `!require_version`, using the `a.b` release format. You can also specify beta versions with the `-beta` suffix. The full supported format is `a.b-betaN`

> [!NOTE]
> Bugfix versions such as `2.0p1` are not supported.
