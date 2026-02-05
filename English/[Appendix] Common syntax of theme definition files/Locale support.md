# Locale support

One of the design principles of CLItheme is locale support. It can automatically detect the current system locale and use the appropriate version of string entries and substitution rules. You can add different language version for a string entry or substitution rule.

## Locale name format

Locale names must be in the form of Linux system locales, such as `zh_CN` and `en_US`. It is composed of two-letter lowercase language name and uppercase region name, separated by an underline.

> [!NOTE]
> Sometimes, the locale name includes an encoding, such a `zh_CN.UTF-8`. **Do not include this suffix** as it might cause locale detection to not work properly.

On Linux and macOS systems, you can use the `locale` terminal command to view the current system locale. Refer to `LANG` and `LANGUAGE` environment variables in the output.

```plaintext
$ locale
LANG="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_CTYPE="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_ALL=
```

On Windows systems, use the command `powershell /c Get-WinSystemLocale` to view the current system locale. Refer to the `Name` column for the locale name.

```plaintext
C:\> powershell /c Get-WinSystemLocale
LCID             Name             DisplayName
----             ----             -----------
1033             en-US            English (United States) 
```

> [!TIP]
> Use locale names with the underscore character. For example, write `en-US` as `en_US` in definition files.

> [!NOTE]
> On newer version of Windows 11, the locale settings of command-line applications is located in the **System locale** setting under **Windows display language** toggle in **Time & Language** section. 
> The **Use Unicode UTF-8 for worldwide language support** option might need to be disabled.

## Adding multiple locale versions for string entries and substitution rules

One core design principle of CLItheme is locale support, so the contents of a string entry or substitution rule are defined in different locale entries, which has the syntax `locale[<Locales>]: <Content>` for single-line and `[locale] <Locales>` for multi-line. For a string entry or substitution rule, applications or `clitheme-exec` will retrieve content from the relevant locale entry based on the current system locale.

It is recommended to add the `default` locale entry (or use `default: <Content>` syntax). If other locale entries in the string entry or substitution rule does not support the current system locale, the `default` entry will be used. If the `default` entry is not defined, the string entry or substitution rule will only be used if other locale entries support the current system locale.

If you don't need to add multi-language support, only defining the `default` locale entry is sufficient.

You can specify multiple locales at the same time, such as `locale[en_US default]:` and `[locale] en_US default`.

```plaintext
{entries}
    [entry] com.example example-app example-entry
        default: Your entry text here
        locale[zh_CN]: 你的定义文本
        # Or use multi-line content blocks
        [locale] en_US
            Your entry text here
        [/locale]
        [locale] zh_CN
            你的定义文本
        [/locale]
    [/entry]
{/entries}
{substrules}
    [subst_string] Some example output message
        default: Output message! :)
        locale[zh_CN]: 输出提示！:)
        # Define entry for both C and en_US locales
        [locale] C en_US
            Output message! :)
        [/locale]
    [/subst_string]
{/substrules}
```

## frontend module: Specifying or disabling locale

When creating a `FetchDescriptor`, the following arguments control locale detection:

- `lang`: Specifies the locale to use when fetching string entries; this entry will override the automatically detected system locale.
    - You can specify multiple space-separated locales (e.g. `en_US zh_CN`); string entries of these locales will be searched in this order
- `disable_lang`: If `True`, disable locale detection and always use the `default` entry for all string entries

```py
from clitheme import frontend

# Using the system locale
f=frontend.FetchDescriptor()

# Using the locale `zh_CN`
f=frontend.FetchDescriptor(lang="zh_CN")

# If the string entry doesn't support `en_US`, try `zh_CN`
f=frontend.FetchDescriptor(lang="en_US zh_CN")

# Always use `default` entries for all string entries
f=frontend.FetchDescriptor(disable_lang=True)
```

## How system locale detection works

The following environment variables are read to determine the list of system locales. Locale entries for string entries and substitution rules are searched based on the following order:

- `LANGUAGE` (May be a list of locales separated with `:`)
    - `en_US` and `en` will be ignored (see [this article](https://wiki.archlinux.org/title/Locale#LANGUAGE:_fallback_locales) for more details)
    - If the list contains `C` or `C.<Encoding>`, `en_US` and `en` will also be processed
    - **This variable will not be read when `LANG` is set to `C`**
- `LC_ALL`
- `LANG`

This order corresponds to the mechianism used by GNU gettext. See [this article](https://www.gnu.org/software/gettext/manual/gettext.html#Locale-Environment-Variables) for more details.

Furthermore, the value of `GetSystemDefaultLocaleName` API call will be queried on Windows systems, which corresponds to the output of `Get-WinSystemLocale` PowerShell command. The original locale name and the name converted to underlines (e.g. `en-US` to `en_US`) will be added to the end of the list.
