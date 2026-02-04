# Defining and using content variables

You can use **content variables** in a theme definition file, making repeated content easier to use and modify.

## Defining variables

To define a content variable, use the `setvar[<Name>]: <Content>` syntax. You can also specify multiple space-separated variables names, like `setvar[var1 var2]:`.

```plaintext
{substrules}
    setvar[example_var]: This is some example content
    # Set both var1 and var2 to "Another example variable"
    setvar[var1 var2]: Another example variable
    # <...>
{/substrules}
# You can define them outside of sections (define globally)
setvar[another_var]: Some variable content
setvar[var3]: Third variable!
```

> [!NOTE]
> Variable contents cannot contain any of the characters in `(){}[]` and cannot be `ESC` (conflicts with the `substesc` feature)

### Variables in global or section scope

Variables defined outside of a section is in the global scope. **Variables defined inside a section is valid for the current section only, including modifications to global variables.** After terminating the section, current variable definitions will revert to what's defined in the global scope.

## Using content variables

To use content variables, [enable](Available%20options.md#How%20to%20set%20options) the `substvar` option and use the `{{<Variable name>}}` syntax in content, such as `{{example_var}}`.

```plaintext
setvar[name]: Vim
(set_options) substvar
{header}
    name: {{name}} theme
    description: A simple theme for {{name}}
{/header}
{substrules}
    [subst_string] {{name}} 9.1
        default: {{name}} Improved 9.1+
    [/subst_string]
{/substrules}
```

You can also use content variables in locale and option fields, such as:

```plaintext
{substrules}
    (enable_subst)
    setvar[langs]: en_US C
    setvar[opts]: strictcmdmatch endmatchhere
    [subst_regex] Some content
        # Equivalent to "locale[en_US C]:"
        locale[{{langs}}]: Substitute content
        # Equivalent to "[locale] en_US C"
        [locale] {{langs}}
            Substitute content
        [/locale]
    # Sets `strictcmdmatch` and `endmatchhere` options
    [/subst_regex] {{opts}}
{/substrules}
```

However, content variables cannot replace syntax phrases:

```plaintext
(set_options) substvar
setvar[place1]: name:
setvar[place2]: version:
{header}
    # Wrong expression:
    # {{place1}} Vim theme
    # {{place2}} 9.1
{/header}
```

> [!NOTE]
> - Variable names cannot contain spaces like `{{var }}` and `{{this and }}` as they will not be processed
> - `{{ESC}}` and character substitution expressions **will not be processed when defining the variable** and is only processed when using them in supported content

## Using content variables when defining another variable

You can use content variables when defining another variable using `setvar`. However, you must enable the `substvar` option before doing so.

```plaintext
setvar[example_one]: Some text here
# No replacement if `substvar` not enabled
# Content: {{example_one}} with another text
setvar[example_two]: {{example_one}} with another text
(set_options) substvar
# Replaces when `substvar` is enabled
# Content: Some text here with another text
setvar[example_three]: {{example_one}} with another text
```
