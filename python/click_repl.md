# click-repl

[![Tests](https://github.com/click-contrib/click-repl/actions/workflows/tests.yml/badge.svg?branch=master)](https://github.com/click-contrib/click-repl/actions/workflows/tests.yml)
[![License](https://img.shields.io/pypi/l/click-repl?label=License)](https://github.com/click-contrib/click-repl/LICENSE)
![Python - version](https://img.shields.io/badge/python-3%20%7C%203.7%20%7C%203.8%20%7C%203.9%20%7C%203.10%20%7C%203.11-blue)
[![PyPi - version](https://img.shields.io/badge/pypi-v0.2.0-blue)](https://pypi.org/project/click-repl/)
![wheels](https://img.shields.io/piwheels/v/click-repl?label=wheel)
![PyPI - Status](https://img.shields.io/pypi/status/click)
![PyPI - Downloads](https://img.shields.io/pypi/dm/click-repl)

# Installation

Installation is done via pip:

```
pip install click-repl
```

# Usage

In your [click](http://click.pocoo.org/) app:

```py
import click
from click_repl import register_repl

@click.group()
def cli():
    pass

@cli.command()
def hello():
    click.echo("Hello world!")

register_repl(cli)
cli()
```

In the shell:

```
$ my_app repl
> hello
Hello world!
> ^C
$ echo hello | my_app repl
Hello world!
```

**Features not shown:**

- Tab-completion.
- The parent context is reused, which means `ctx.obj` persists between
  subcommands. If you're keeping caches on that object (like I do), using the
  app's repl instead of the shell is a huge performance win.
- `!` - prefix executes shell commands.

You can use the internal `:help` command to explain usage.

# Advanced Usage

For more flexibility over how your REPL works you can use the `repl` function
directly instead of `register_repl`. For example, in your app:

```py
import click
from click_repl import repl
from prompt_toolkit.history import FileHistory

@click.group()
def cli():
    pass

@cli.command()
def myrepl():
    prompt_kwargs = {
        'history': FileHistory('/etc/myrepl/myrepl-history'),
    }
    repl(click.get_current_context(), prompt_kwargs=prompt_kwargs)

cli()
```

And then your custom `myrepl` command will be available on your CLI, which
will start a REPL which has its history stored in
`/etc/myrepl/myrepl-history` and persist between sessions.

Any arguments that can be passed to the [`python-prompt-toolkit`](https://github.com/prompt-toolkit/python-prompt-toolkit) [Prompt](http://python-prompt-toolkit.readthedocs.io/en/stable/pages/reference.html?prompt_toolkit.shortcuts.Prompt#prompt_toolkit.shortcuts.Prompt) class
can be passed in the `prompt_kwargs` argument and will be used when
instantiating your `Prompt`.
