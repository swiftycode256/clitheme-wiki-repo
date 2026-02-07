# Options in definition files

This article details what options can be set in theme definition files and how to use and set them.

## How to set options

You can use `(set_options) <Options>` to set options globally. You can specify multiple space-separated options, such as `(set_options) substesc substvar leadtabs:1`. Afterwards, these options will be applied on future definitions.

You can also set options for individual blocks by adding them after their end phrase. For example: `[/subst_regex] endmatchhere` or `[/default] substesc substvar leadtabs:1`

To set options for single-line definitions, use line boundaries (enable the `linebounds` option) and add them at the end of the content, such as `default: |Content {{var}}| substvar`.

```plaintext
{substrules}
    setvar[tot]: (ToT)/~~~
    # `substesc` and `endmatchhere` options will be effective for following substitution rules
    (set_options) endmatchhere substesc
    [subst_string] Some other text {{ESC}}
        [default]
            Another good stuff {{tot}}
        [/default] substvar
    # Disable endmatchhere and enable foregroundonly for this definition
    [/subst_string] noendmatchhere foregroundonly
    (set_options) linebounds
    # Use line boundaries to set options for single-line content
    <filter_cmd> |example-app| strictcmdmatch
    [subst_regex] Error at file number (?P<num>\d+): item (?P<name>.+) not found
        # Enable `substvar` for the following content
        locale[zh_CN]: |{{tot}} 在文件\g<num>发生错误：找不到项目"\g<name>"！(>﹏<)| substvar
        default: |{{tot}} Error at file number \g<num>! Could not find item "\g<name>"!| substvar
    [/subst_regex]
{/substrules}
```

### Options set globally and in sections

Options are set globally when `(set_options)` are used outside of any section. Options set in a section are only valid for the current section only. After ending the section, the option settings are reverted to the globally defined options.

## List of options

### Indents in multi-line content blocks

The following options are applicable to multi-line content blocks except list content:

- `leadtabindents:<n>`: Add `<n>` tab indents to the start of each line
- `leadspaces:<n>`: Add `<n>` spaces to the start of each line

`<n>` should be an integer number and spaces are not allowed between the name and value.

#### Example:

```plaintext
[entry] entry2
    [default]
        Text 1
        Text 2
            Text 3
    [/default] leadtabs:1
[/entry]
```

Value:

```plaintext
    Text 1
    Text 2
        Text 3
```

### Content substitution

The following options are applicable to most text content and multi-line content blocks. Use `no<Option>` to disable them, such as `nosubstvar`.

- `substvar`: Replace content variable references in content with the variable content; see [Content variables](Content%20variables.md) for more information
- `linebounds`: Support adding line boundary character `|` at the start and end of content, allowing leading and trailing spaces to be inserted
    - When this option is enabled, all content starting with `|` must end with this character, or else syntax error occurs
    - **Some fields like path names do not preserve leading and trailing spaces even if line boundaries are used**

```plaintext
(set_options) linebounds
# Content: "Line content  "
[subst_regex] |Line content  |
    # Content: "  Replace content|new content "
    default: |  Replace content|new content |
    # Line boundaries can also be used in multi-line content blocks
    [default]
        |Content  |
    [/default]
[/subst_regex]
```

The following options are only applicable for match & substitute content in `substrules`, content of string entries in `entries`, and file content in `manpages`:

- `substesc`: Replace `{{ESC}}` in content with the ASCII Escape character (`0x1B`)
- `substchar`: Use hexadecimal values to insert special characters, supporting `{{[xXX]}}`, `{{[uXXXX]}}`, and `{{[UXXXXXXXX]}}` formats (similar to Python strings)

> [!TIP]
> You can use `(enable_subst)` to enable all content substitution options, and use `(disable_subst)` to disable them:
>
> ```plaintext
> # Same as `(set_options) substvar substesc substchar linebounds`
> (enable_subst)
> setvar[var2]: |Some content {{var}} {{ESC}} {{[U00030edd]}}|
> # Same as `(set_options) nosubstvar nosubstesc nosubstchar nolinebounds`
> (disable_subst)
> ```

### Substitution rules

The following options are applicable to `[subst_string]` and `[subst_regex]` blocks:

- ~~`subststdoutonly`: Only match content from standard output~~
- ~~`subststderronly`: Only match content from standard error~~
- ~~`substallstreams`: Used to unset the above options~~
- `nlmatchcurpos`: For substitution rules with multi-line match patterns, the newlines in its content will also match cursor position sequences (e.g. `ESC` `[` `5;1H`)
    - It does not apply to regex expressions like `\n` and character substitutions like `{{[x0a]}}`
    - **The replacement content will still use newlines.** Therefore, ensure that the sequence is only used for newlines instead of moving to other positions.
- `endmatchhere`: If the substitution rules matches the output, remaining substitution rules in current file will not be matched in the affected lines
    - Use `noendmatchhere` to unset
- `foregroundonly` (**Only effective on Unix systems**): Apply substitution rules only the process is in foreground state. When most shells are running other commands, it exits the foreground state. This option can prevent accidental matches when running shells through `clitheme-exec`.
    - Use `noforegroundonly` to unset
    - See when the process changes its foreground state by running it with `clitheme-exec --foreground-stat`
    - You can also specify this option in a command filter (such as `[/filter_cmds] foregroundonly`) for applying it to its substitution rules. When the command filter is unset or changed, the option will revert to what's set before it, **even if it is changed mid-way**.

> [!NOTE]
> - Most shells doesn't change their foreground status when executing builtin commands
> - Some shells such as `bash`, `zsh`, and `csh` change their foreground status when outputting command execution errors such as `command not found` and `permission denied`:
> 
> ```plaintext
> $ clitheme-exec --foreground-stat bash
> bash-3.2$ wef
> ! Foreground: False
> bash: wef: command not found
> ! Foreground: True
> bash-3.2$
> ```

### Command filters

The following options are applicable to `<filter_cmd>` and `[filter_cmds]`.

- `strictcmdmatch`: The target command must start with the command filter
    - For example, the target command `command --get thing` matches the filter `command --get thing --another`, but not `command --another thing --get`
- `exactcmdmatch`: The target command must exactly equal the command filter
- `smartcmdmatch`：Split command options starting with one `-` into individual options (e.g. process `-rfc` as `-r -f -c`)
    - For example, the target command `rm -rf folder` matches the filter `rm -r` and `rm -f`, and the target command `rm -r -f folder` matches the filter `rm -rf`
    - Options starting with multiple dashes (`--`) are not processed
- `normalcmdmatch`: **Default options used for resetting**; the target command must include all phrases in the command filter
    - For example, `example-app --this --that` matches the filter `example-app --that`
