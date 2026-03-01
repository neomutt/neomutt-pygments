# Pygments Lexer for NeoMutt Configuration Files

A [Pygments](https://pygments.org/) lexer plugin for
[NeoMutt](https://neomutt.org/) configuration files (`neomuttrc` / `muttrc`).

This lexer is used by NeoMutt's documentation on
[Read the Docs](https://docs.neomutt.org/) to provide syntax highlighting for
configuration examples.

## What It Supports

The lexer highlights the following NeoMutt configuration elements:

- **Commands** — `set`, `unset`, `toggle`, `reset`, `bind`, `macro`, `color`,
  `alias`, `source`, `push`, `exec`, `ifdef`, and many more
- **Hooks** — `folder-hook`, `send-hook`, `message-hook`, `startup-hook`,
  `shutdown-hook`, `timeout-hook`, etc.
- **Color definitions** — named colors (`red`, `brightcyan`), palette colors
  (`color0`–`color255`), and RGB hex colors (`#ff0000`)
- **Color objects** — `normal`, `error`, `indicator`, `status`, `sidebar_new`,
  `body`, `header`, `index`, quoted levels, and more
- **Key bindings** — menu names, key sequences, and function names
  (`<next-page>`, `<save-message>`)
- **Config variables** — option names in `set` commands, `$variable` expansion
- **Strings** — double-quoted, single-quoted, and backtick (command
  substitution) strings with escape sequence highlighting
- **Operators** — `=`, `+=`, `-=`, `?`, `&`, `!`
- **Enum values** — quad options (`yes`, `no`, `ask-yes`, `ask-no`) and sort
  values (`date`, `threads`, `reverse-date`, etc.)
- **Comments** — full-line and trailing `#` comments
- **Alias tag annotations** — `# tags:vip,work` comments on alias lines
- **Line continuations** — backslash-newline handling
- **Mono attributes** — `bold`, `underline`, `reverse`, `standout`, `italic`

## Installation

```sh
pip install .
```

Or in development/editable mode:

```sh
pip install -e .
```

This registers the lexer as a Pygments plugin.  Once installed, Pygments will
automatically discover it via the `pygments.lexers` entry point.

## Usage

### Command Line

Highlight a NeoMutt config file in the terminal (256-color):

```sh
pygmentize -l neomutt -f terminal256 tests/example.neomuttrc
```

Generate a syntax-highlighted HTML file:

```sh
pygmentize -l neomutt -f html -O full,style=monokai -o output.html tests/example.neomuttrc
```

Generate an SVG image:

```sh
pygmentize -l neomutt -f svg -o output.svg tests/example.neomuttrc
```

### Automatic File Detection

Pygments will auto-detect the lexer for files matching these patterns:

- `*.neomuttrc`, `*.muttrc`
- `neomuttrc`, `muttrc`
- `.neomuttrc`, `.muttrc`

So you can simply run:

```sh
pygmentize -f terminal256 ~/.neomuttrc
```

### In Python

```python
from pygments import highlight
from pygments.formatters import HtmlFormatter
from neomutt_lexer import NeoMuttLexer

code = 'set sort = reverse-threads\ncolor normal white default\n'
result = highlight(code, NeoMuttLexer(), HtmlFormatter())
print(result)
```

### In Sphinx / Read the Docs

The NeoMutt documentation at <https://docs.neomutt.org/> uses this lexer for
syntax-highlighted configuration examples.  In a Sphinx project, once the
package is installed, use the `neomutt` language in code blocks:

````rst
.. code-block:: neomutt

   set sort = reverse-threads
   color normal white default
   bind index j next-entry
````

## Running Tests

```sh
pip install -e .
pytest
```

Or with verbose output:

```sh
pytest -v
```

## License

Copyright © 2026 Richard Russon (FlatCap)

This program is free software: you can redistribute it and/or modify it under
the terms of the **GNU General Public License** as published by the Free
Software Foundation, either version 3 of the License, or (at your option) any
later version.

See [LICENSE](LICENSE) for the full text.
