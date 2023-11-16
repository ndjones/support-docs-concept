# Title (H1)

```md
# Title (H1)
```

## H2

```md
## H2
```

### H3

```md
### H3
```

#### H4

```md
#### H4
```

Headers should have a blank line before and after.

'H1' (`#`) is for the page title. Setting a title here will change it in the nav also.

See [Title](#title) for more info.

### Text Emphasis

**bold**: `**bold**`

_italic_: `_italic`

### Tab Containers

=== "Tab One"
    someting in the tab
=== "Tab two"
    something else

```md
=== "Tab One"
    someting in the tab
=== "Tab two"
    something else
```

### Admonations

!!! note
    This is a test admonation.

```md
!!! note
    This is a test admonation.
```
!!! tip
    This is a test admonation.

```md
!!! tip
    This is a test admonation.
```
!!! info
    This is a test admonation.

```md
!!! info
    This is a test admonation.
```
!!! question
    This is a test admonation.

```md
!!! question
    This is a test admonation.
```
!!! warning
    This is a test admonation.

```md
!!! warning
    This is a test admonation.
```
!!! failure
    This is a test admonation.

```md
!!! failure
    This is a test admonation.
```
!!! danger
    This is a test admonation.

```md
!!! danger
    This is a test admonation.
```
!!! bug
    This is a test admonation.

```md
!!! bug
    This is a test admonation.
```
!!! example
    This is a test admonation.

```md
!!! example
    This is a test admonation.
```
!!! quote
    This is a test admonation.

```md
!!! quote
    This is a test admonation.
```

Any admonation can be made collapsable by replacing the `!!!` with `???` (closed), or `???+` (open)

_Note for future, once decided which of these we will use, remove the others. And give description of when to use._

### Code

Code blocks require a language lexxer in order to do syntax hilighting, e.g. `python` ,`slurm`, `cuda`, `json`, `md`.
[A full list of lexxers can be found in this list](https://pygments.org/languages/).

For plain code blocks, still good to use a class as descriptor (e.g. `txt`, `stdout`, `stderr`).
May want to add formatting to this later.

<pre><code><span>```stdout</span>
<span>some code</span>
<span>```</span>
</code></pre>

```stderr
some code 
```

#### Block

```py
import somepackage

formatting = True
if formatting:
    Print(formatting) # A comment
```

<pre><code><span>```py</span>
<span>import somepackage</span>
<span></span>
<span>formatting = True</span>
<span>if formatting:</span>
<span>  Print(formatting) # A comment</span>
<span>```</span>
</code></pre>

#### Inline

This is some `echo "Inline Code"`.

<pre><code><span>This is some `echo "Inline Code"`.</span></code></pre>

#### Keyboard

Keyboard keys can be added using the `<kbd>` tag.

Press <kbd>ctrl</kbd> + <kbd>c</kbd> to copy text from terminal.

```md
Press <kbd>ctrl</kbd> + <kbd>c</kbd> to copy text from terminal.
```

Note the additional spacing around the `+` else it will appear cramped.

### Images

```md
![This is an image]("assets/images/FENSAP_GUI1.png")
```

![This is an image]("assets/images/FENSAP_GUI1.png")

### Links

Try to avoid putting links on ambiguous words, e.g.

=== "Bad"
    View the software homepage [here](www.example.com).

    ```md
    View the homepage [here](www.example.com).
    ```

=== "Better"
    View the [software homepage](www.example.com).

    ```md
    View the [software homepage](www.example.com).
    ```

[External Link]("https://example.com")

```md
[External Link]("https://example.com")

```

[Internal Link]("General/Announcements")

```md
[Internal Link]("General/Announcements")

```

[Anchor Link](#links)

```md
[Anchor Link](#links)

```

### Tooltips

[Hover over me](https://example.com "I'm a link with a custom tooltip.")

```md
[Hover over me](https://example.com "I'm a link with a custom tooltip.")
```

Acroynym should be automatically tooltipped e.g. MPI.

```md
Acroynym should be automatically tooltipped e.g. MPI.
```

### Tables

[Markdown Table Generator](https://www.tablesgenerator.com/markdown_tables), is a handy tool for complex tables/

Tables can be constructed using `|` to seperate colums, and `--` to designate headers.

Number of dashes has no effect, things dont have to be lined up when in markdown, just looks nice.
Leading and trailing `|` are optional.

 Head  | Head
-------|-------
Thing1 | Thing2
Thing3 | Thing3

```md
 Head | Head
 --- | -----------
 Thing1 | Thing2
 Thing3 | Thing 3
```

`:`'s can be used to align tables.

| Left      | Center    | Right     |
| :---      |    :----: |---:       |
| Words     | Words     | Words     |
| Words     | Words     | Words     |

```md
| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |
```