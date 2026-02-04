# Multi-line content block

Some definitions support using multi-line content blocks to define its contents. It allows you to define content with multiple lines or names containing spaces.

> [!TIP]
> If you want to insert the end phrase (e.g. `[/locale]`) at the start of a line, add the escape character `\` before it. It must be placed at the start of the line, or the escape character will not be removed.

```plaintext
[default]
    Use 
    \[/default]
    To end the block
    Using [/default]
    will also work
[/default]
```

> [!IMPORTANT]
> If there are no empty lines at the end of the block, newline characters will not be added at the end of the content.

## How indents and edge spaces are processed

The common length of indents of all lines in a content block are deducted from each line (tab characters equal 8 spaces). This mean that only the difference of indents between lines are kept. If you want to add common indents for all lines, you can [specify](Available%20options.md#How%20to%20set%20options) the `leadtabs:<n>` or `leadindents:<n>` options; the former adds `<n>` tab characters, while the latter adds `<n>` spaces as indents.

Trailing spaces in the content block are ignored. To add trailing spaces, [enable](Available%20options.md#How%20to%20set%20options) the `linebounds` option and place line boundaries in a line, such as `|line content    |`.

> [!NOTE]
> For certain list-like content blocks (like `[supported_apps]` and `[filter_cmds]`), no edge spaces and indents are preserved and indenting options are not allowed.

### Example

```plaintext
{entries}
    [entry] entry1
        [default]
            Text 1
            Text 2
                Text 3
                    Text 4
        [/default]
    [/entry]
    [entry] entry2
        [default]
            Text 1
            Text 2
                Text 3
        [/default] leadtabs:1
    [/entry]
    [entry] entry3
        [default]
    Text 1
    Text 2
    Text 3
        [/default]
    [/entry]
{/entries}
```

Corresponds to the following content:

| `entry1` | `entry2` | `entry3` |
| --- | --- | --- |
| <pre>Text 1<br>Text 2<br>    Text 3<br>        Text 4</pre> | <pre>    Text 1<br>    Text 2<br>        Text 3</pre> | <pre>Text 1<br>Text 2<br>Text 3</pre>
