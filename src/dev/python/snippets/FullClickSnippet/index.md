# Click

Quelques *snippets* d'utilisation de click

## Requirements

```ini
click==8.*
PyYAML==6.*
pytoml==0.1.21
```

## Basic

Utilisation d'un argument *positional* avec l'aide en ligne.

```python
#!/usr/bin/env python3
"""Basic click demo."""

import click


@click.command(
    context_settings={"help_option_names": ["-h", "--help"]},
    help="Basic demo.",
)
@click.argument(
    "name",
    type=click.STRING,
    metavar="NAME ",
)
def main(name: str):
    """Main function."""
    click.clear()
    click.echo(f"Hello {name} (classic)")
    click.secho(f"Hello {name} (fg only)", fg="green")
    click.secho(f"Hello {name} (bg only)", bg="yellow")
    click.secho(f"Hello {name} (both fg & bg)", fg="red", bg="blue")


if __name__ == "__main__":
    main()  # pylint: disable=E1120

```

## Prompt

Si un parametre est manquant, on utilise un prompt:

```python
#!/usr/bin/env python3
"""Use prompt for parameter."""

import click


@click.command(
    context_settings={"help_option_names": ["-h", "--help"]},
    help="Basic demo 2: Use prompt if arguments is missing.",
)
@click.option("-n", "--name", prompt="Your name", help="The person to greet.")
@click.option("-c", "--count", default=1, help="number of greetings")
def main(name: str, count: int):
    """Simple program that greets NAME for a total of COUNT times."""
    for _ in range(count):
        click.echo(f"Hello, {name}!")


if __name__ == "__main__":
    main()  # pylint: disable=E11
```
## Parametre du type `Path`

On utilise un parametre dont les proprietes seront verifier mais dont le resultats sera un `string` pour permetre de créér un *Path*

```python
#!/usr/bin/env python3
"""Parse and display data with click.file."""

from typing import Dict, List
from string import ascii_letters
import math
from pathlib import Path

from collections import Counter
import click


def convert_size(size_bytes):
    """Convert size in human readable form."""
    if size_bytes == 0:
        return "0B"
    size_name = ("B", "KB", "MB", "GB", "TB")
    i = int(math.floor(math.log(size_bytes, 1024)))
    if i >= len(size_name):
        return "0B"
    p = math.pow(1024, i)
    s = round(size_bytes / p, 2)
    return f"{s} {size_name[i]}"


@click.command(
    context_settings={"help_option_names": ["-h", "--help"]},
    help="Basic demo 3: Use click.Path arguments.",
)
@click.argument(
    "folder_path",
    type=click.Path(exists=True, file_okay=False, dir_okay=True),
    metavar="MY_DIRECTORY",
)
@click.argument(
    "file_path",
    type=click.Path(exists=True, file_okay=True, dir_okay=False),
    metavar="MY_FILE",
)
def main(folder_path: str, file_path: str):
    """Main function."""
    click.clear()

    folder_path = Path(folder_path)
    file_path = Path(file_path)

    # Display directory
    files: List[Path] = folder_path.glob("*.py")
    files_details: List[str] = [
        f"{x.stem} ({convert_size(x.stat().st_size)})" for x in files
    ]
    click.secho("Glob folder", bg="green")
    click.echo("\n".join(files_details))

    # File
    contains: str = file_path.read_text()  # pylint: disable=W1514
    stat: Dict[str, int] = Counter(contains)
    letter_stat = {k: v for k, v in stat.items() if k in ascii_letters}
    letter_stat = sorted(letter_stat.items(), key=lambda x: x[1], reverse=True)
    click.secho(f"Letters statstics for {file_path.name}", bg="green")
    click.echo("\n".join(f"{x[0]} => {x[1]}" for x in letter_stat))


if __name__ == "__main__":
    main()  # pylint: disable=E1120

```

## Parametre **FILE**

Parametre de type `FILE`. *Handle* en paramtre.

```python
#!/usr/bin/env python3
"""Read JSON file with Click.file."""

from typing import IO
from json import load
import click


@click.command(
    context_settings={"help_option_names": ["-h", "--help"]},
    help="Basic demo 4: Use click.File arguments.",
)
@click.argument("json_file", type=click.File(mode="r", encoding="utf-8"))
def main(json_file: IO[str]):
    """Main function."""
    click.clear()
    json_contains = load(json_file)
    click.secho("Json contains", bg="green")
    click.echo(json_contains)


if __name__ == "__main__":
    main()  # pylint: disable=E1120

```

## Parametre Enum/Choice

Utilisation d'un `Enum` custom pour permettre d'exploiter un *choice*

```python
#!/usr/bin/env python3
""" Parse file according with parser."""
from typing import IO, Optional

from enum import Enum

import json
import csv
import yaml

import click


class ConfigFormat(Enum):
    """Enum format type."""

    YAML = 0
    JSON = 1
    CSV = 2
    TXT = 3


class EnumType(click.Choice):
    """Helper to ;ap Enum <=> click.Choice."""

    def __init__(self, enum):
        self.__enum = enum
        super().__init__(enum.__members__)

    def convert(self, value, param, ctx):
        return self.__enum[super().convert(value, param, ctx)]


@click.command(
    context_settings={"help_option_names": ["-h", "--help"]},
    help="Basic demo 5: Use click.Choice arguments.",
)
@click.argument("file_path", type=click.File(mode="r", encoding="utf-8"))
@click.option(
    "-f",
    "--form",
    type=EnumType(ConfigFormat),
    default=ConfigFormat.JSON.name,
    show_default=True,
    help="Select format type of input file",
)
def main(file_path: IO[str], form: ConfigFormat):
    """Parse data from file according with format."""

    click.clear()
    contains: Optional[str] = None

    if form == ConfigFormat.JSON:
        contains = json.load(file_path)
    elif form is ConfigFormat.YAML:
        contains = yaml.safe_load(file_path)
    elif form == ConfigFormat.CSV:
        spamreader = csv.reader(file_path, delimiter=" ", quotechar="|")
        contains = ""
        for row in spamreader:
            contains += f"{', '.join(row)}\n"
    else:
        contains = "\n".join(file_path.readlines())

    click.secho("File contains", bg="green")
    click.echo(contains)


if __name__ == "__main__":
    main()  # pylint: disable=E1120

```

## Utilidation des `group`

```python
#!/usr/bin/env python
""" Display some usefull tools available by group."""

import time
import click


@click.group()
def cli():
    """Examples of usefull click utils."""


@cli.command()
def editor():
    """Launch an editor."""
    message = click.edit("Prepopulated text")
    click.echo(f"Your message: {message}")


@cli.command(name="find-app-dir")
def find_app_dir():
    """Find the appropriate application data folder."""
    click.echo(click.get_app_dir("part05"))


@cli.command()
@click.option("--lines", default=7, show_default=True)
def paging(lines: int):
    """Page through output."""
    data = "\n".join([f"Line {lines}"])
    click.echo_via_pager(data)


@cli.command(name="progress-bar")
@click.option(
    "--delay",
    help="Step progression delay.",
    required=True,
    default=0.5,
    show_default=True,
)
@click.option("--count", help="", default=10)
def progress_bar(count: int, delay: float):
    """Display a progress bar."""
    data = range(count)
    with click.progressbar(data) as click_bar:
        for _ in click_bar:
            time.sleep(delay)


if __name__ == "__main__":
    cli()  # pylint: disable=E1120

```