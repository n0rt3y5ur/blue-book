---
title: Typer
date: 20220906
author: Lyz
---

[Typer](https://typer.tiangolo.com/) is a library for building CLI applications
that users will love using and developers will love creating. Based on Python
3.6+ type hints.

The key features are:

* *Intuitive to write*: Great editor support. Completion everywhere. Less time
    debugging. Designed to be easy to use and learn. Less time reading docs.
* *Easy to use*: It's easy to use for the final users. Automatic help, and
    automatic completion for all shells.
* *Short*: Minimize code duplication. Multiple features from each parameter
    declaration. Fewer bugs.
* *Start simple*: The simplest example adds only 2 lines of code to your app:
    1 import, 1 function call.
* *Grow large*: Grow in complexity as much as you want, create arbitrarily
    complex trees of commands and groups of subcommands, with options and
    arguments.

# Installation

```bash
pip install 'typer[all]'
```

# [Usage](https://typer.tiangolo.com/#the-absolute-minimum)

Create a `typer.Typer()` app, and create two subcommands with their parameters.

```python
import typer

app = typer.Typer()


@app.command()
def hello(name: str):
    print(f"Hello {name}")


@app.command()
def goodbye(name: str, formal: bool = False):
    if formal:
        print(f"Goodbye Ms. {name}. Have a good day.")
    else:
        print(f"Bye {name}!")


if __name__ == "__main__":
    app()
```

## [Using subcommands](https://typer.tiangolo.com/tutorial/subcommands/single-file/)


In some cases, it's possible that your application code needs to live on a single file.

```python
import typer

app = typer.Typer()
items_app = typer.Typer()
app.add_typer(items_app, name="items")
users_app = typer.Typer()
app.add_typer(users_app, name="users")


@items_app.command("create")
def items_create(item: str):
    print(f"Creating item: {item}")


@items_app.command("delete")
def items_delete(item: str):
    print(f"Deleting item: {item}")


@items_app.command("sell")
def items_sell(item: str):
    print(f"Selling item: {item}")


@users_app.command("create")
def users_create(user_name: str):
    print(f"Creating user: {user_name}")


@users_app.command("delete")
def users_delete(user_name: str):
    print(f"Deleting user: {user_name}")


if __name__ == "__main__":
    app()
```

Then you'll be able to call each subcommand with:

```bash
python main.py items create
```

For more complex code use [nested subcommands](#nested-subcommands)

### [Nested Subcommands](https://typer.tiangolo.com/tutorial/subcommands/nested-subcommands/)

You can split the commands in different files for clarity once the code starts
to grow:

!!! note "File: `reigns.py`"

    ```python
    import typer

    app = typer.Typer()


    @app.command()
    def conquer(name: str):
        print(f"Conquering reign: {name}")


    @app.command()
    def destroy(name: str):
        print(f"Destroying reign: {name}")


    if __name__ == "__main__":
        app()
    ```

!!! note "File: `towns.py`"

    ```python
    import typer

    app = typer.Typer()


    @app.command()
    def found(name: str):
        print(f"Founding town: {name}")


    @app.command()
    def burn(name: str):
        print(f"Burning town: {name}")


    if __name__ == "__main__":
        app()
    ```

!!! note "File: `lands.py`"

    ```python
    import typer

    import reigns
    import towns

    app = typer.Typer()
    app.add_typer(reigns.app, name="reigns")
    app.add_typer(towns.app, name="towns")

    if __name__ == "__main__":
        app()
    ```

!!! note "File: `users.py`"

    ```python
    import typer

    app = typer.Typer()


    @app.command()
    def create(user_name: str):
        print(f"Creating user: {user_name}")


    @app.command()
    def delete(user_name: str):
        print(f"Deleting user: {user_name}")


    if __name__ == "__main__":
        app()
    ```

!!! note "File: `items.py`"

    ```python
    import typer

    app = typer.Typer()


    @app.command()
    def create(item: str):
        print(f"Creating item: {item}")


    @app.command()
    def delete(item: str):
        print(f"Deleting item: {item}")


    @app.command()
    def sell(item: str):
        print(f"Selling item: {item}")


    if __name__ == "__main__":
        app()
    ```

!!! note "File: `main.py`"

    ```python
    import typer

    import items
    import lands
    import users

    app = typer.Typer()
    app.add_typer(users.app, name="users")
    app.add_typer(items.app, name="items")
    app.add_typer(lands.app, name="lands")

    if __name__ == "__main__":
        app()
    ```
## [Using the context](https://typer.tiangolo.com/tutorial/commands/context/)

When you create a Typer application it uses [`Click`](click.md) underneath. And
every Click application has a special object called a "Context" that is normally
hidden.

But you can access the context by declaring a function parameter of type
`typer.Context`.

The context is also used to store objects that you may need for all the
commands, for example a [repository](repository_pattern.md). [Although this
doesn't seem to be supported yet by
`typer`](https://github.com/tiangolo/typer/issues/232).

## [Using short option names](https://typer.tiangolo.com/tutorial/options/name/)

```python
import typer


def main(user_name: str = typer.Option(..., "--name", "-n")):
    print(f"Hello {user_name}")


if __name__ == "__main__":
    typer.run(main)
```

The `...` as the first argument is to make the [option required](https://typer.tiangolo.com/tutorial/options/required/)

## [Create `-vvv`](https://typer.tiangolo.com/tutorial/parameter-types/number/#counter-cli-options)

You can make a CLI option work as a counter with the `counter` parameter:

```python

import typer


def main(verbose: int = typer.Option(0, "--verbose", "-v", count=True)):
    print(f"Verbose level is {verbose}")


if __name__ == "__main__":
    typer.run(main)
```




# References

* [Docs](https://typer.tiangolo.com/)