# Writing definition files: String entries

This article will detail how to define string entries in a [theme definition file](../[Appendix]%20Common%20syntax%20of%20theme%20definition%20files/File%20format%20and%20syntax).

## Writing the `header` section

First, you need to define some basic information about the theme and file, such as name and description. Please see the [header section](../[Appendix]%20Common%20syntax%20of%20theme%20definition%20files/header%20section) article.

## Writing string entries

String entries are defined in the `entries` section:

```plaintext
{entries}
    # <Define string entries here>
{/entries}
```

### Using the `[entry]` block

To define a string entry, use the `[entry]` block and include the [path name](./Developer%20docs/1.%20Path%20names%20and%20data%20structure) of the entry, such as `[entry] com.example example-app text-one`.

One core design principle of CLItheme is [locale support](../[Appendix]%20Common%20syntax%20of%20theme%20definition%20files/Locale%20support), so the contents of a string entry are defined in different locale entries, which has the syntax `locale[<Locales>]: <Content>`. For a string entry, applications will retrieve the string content from the relevant locale entry based on the current system locale.

Locale names must be in the Linux system locale format, such as `zh_CN` and `en_US`. For more information, please see [Locale support](../[Appendix]%20Common%20syntax%20of%20theme%20definition%20files/Locale%20support).

You can specify multiple locales at the same time, such as `locale[en_US default]:`.

It is recommended to add the `default` locale entry (or use `default: <Content>` syntax). If other locale entries in the string entry does not support the current system locale, the `default` entry will be used. If the `default` entry is not defined, the string entry will only be used if other locale entries support the current system locale.

If you don't need to add multi-language support to applications, only defining the `default` locale entry is sufficient.

```plaintext
{entries}
    [entry] com.example example-app text-one
        default: Some text
        locale[en_US]: Some text
        locale[zh_CN]: 一些文本
        # You can also specify multiple locales:
        locale[en_US default]: Some text
    [/entry]
{/entries}
```

> [!TIP]
> Please see [Available options](../[Appendix]%20Common%20syntax%20of%20theme%20definition%20files/Available%20options) for more options and features like line boundaries and content variables.

### Multi-line content blocks

You can use multi-line content blocks when defining string entries, which start with the `[locale] <Locale>` syntax. You can also specify multiple space-separated locales at the same time.

You can use the `[default]` syntax to define the `default` locale entry.

```plaintext
{entries}
    [entry] example-entry
        # You can define multiple locales:
        [locale] en_US default
            This is an entry
            Another line like that
            A new line
        [/locale]
        # Equivalent to `[locale] default`:
        [default]
            This is an entry
            Another line like that
            A new line
        [/default]
    [/entry]
{/entries}
```

When `example-entry` is fetched, it returns:

```plaintext
This is an entry
Another line like that
A new line
```

> Please see [Multi-line content block](../[Appendix]%20Common%20syntax%20of%20theme%20definition%20files/Multi-line%20content%20block) for how indents are processed and other information.

### Using `<in_domainapp>` and `<in_subsection>`

You can use `<in_domainapp>` and `<in_subsection>` to reduce repetition in [path names](./Developer%20docs/1.%20Path%20names%20and%20data%20structure). After defining these parameters, they will be added in front of all path names in `[entry]` for subsequent string entry definitions, with the format of `<domain> <app> <subsections> [Path]`.

`<in_domainapp>` has the syntax of `<in_domainapp> [domain] [app]`, and only 2 space-separated phrases can be specified. Multiple space-separated phrases (subsection names) can be specified after `<in_subsection>`.

Use `<unset_domainapp>` and `<unset_subsection>` to unset these parameters.

> [!IMPORTANT]
> Using `<in_domainapp>` and `<unset_domainapp>` will also unset any subsection parameters previously defined with `<in_subsection>`.

```plaintext
{entries}
    <in_domainapp> com.example example-app
        # Same as "com.example example-app entry-one"
        [entry] entry-one
            default: Some text
        [/entry]

        <in_subsection> subsection-one subsection-two
            # Same as "com.example example-app subsection-one subsection-two entry-two"
            [entry] entry-two
                default: Some text
            [/entry]

        <in_subsection> subsection-three
            # Same as "com.example example-app subsection-three entry-three"
            [entry] entry-three
                default: Some text
            [/entry]
    # Will also unset previously defined subsection parameters
    <unset_domainapp>
{/entries}
```

### Defining multiple path names at once

When the content of multiple string entries are the same, you can specify multiple path names at once, eliminating the need for duplicating content and making changes easier. Specify multiple `[entry]` phrases on adjacent lines:

```plaintext
{entries}
    [entry] entry-one
    [entry] another-entry
        default: Some string definition content
    [/entry]
    # Equals the following:
    [entry] entry-one
        default: Some string definition content
    [/entry]
    [entry] another-entry
        default: Some string definition content
    [/entry]
{/entries}
```

## Things to note when writing string entries

If the string entries doesn't work properly in applications, try the following:

- Keep all phrases used for functions like `scanf` and `format`, such as `%s` and `{0}`
- Avoid using expressions like `\n` or `\r` as they will not be automatically converted
