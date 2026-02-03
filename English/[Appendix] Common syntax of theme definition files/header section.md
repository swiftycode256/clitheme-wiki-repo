# The `header` section

In every theme definition file, it is required that some basic information are defined in the `header` section for the `clitheme show-info` command.

```plaintext
{header}
    # <...>
{/header}
```

The following information entries can be defined in the `header` section. The only required entry is `name`, but others are recommended.

- `name`: Define the name of the theme
- `description`: Define a description for the theme
- `version`: Define the theme's version, which can be used for tracking revisions like `1.0`
- `locales`: Define which locales are supported by this theme, containing space-separated phrases (e.g. `en_US zh_CN`)
- `supported_apps`: Define which applications are supported by this theme, containing space-separated phrases (e.g. `example-app another-example-app`)

```plaintext
{header}
    name: example-app theme
    description: A theme created for example-app
    version: 1.0
    locales: zh_CN en_US
    supported_apps: example-app another-example-app
{/header}
```

## Multi-line content block

You can use [multi-line content blocks](Multi-line%20content%20block.md) for the following entries:

- `description`: with `[description]` and `[/description]`
- `supported_apps`: with `[supported_apps]` and `[/supported_apps]`
- `locales`: with `[locales]` and `[/locales]`

In `[supported_apps]` and `[locales]`, phrases are line-separated instead of space-separated, which allows you to insert spaces for each element.

```plaintext
{header}
    name: Example theme
    [description]
        Some description here
        Another description here
    [/description]
    [locales]
        Simplified Chinese (zh_CN)
        English (en_US)
    [/locales]
    [supported_apps]
        App one
        App two
        App three
    [/supported_apps]
{/header}
```

The following information is displayed with `clitheme show-info`:

```plaintext
$ clitheme show-info
[1] Example theme
Description:
Some description here
Another description here
Supported locales:
• Simplified Chinese (zh_CN)
• English (en_US)
Supported apps:
• App one
• App two
• App three
```
