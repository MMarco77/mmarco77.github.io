# Get and Post

Simple petit script pour résoudre un challenge mis à dispo. par un server.

## Server

```python
#!/usr/bin/env python3
"""
TCP challenge helper
"""

from typing import Optional
import socket
from functools import partial

import click

# ------------------------- [SERVER] -------------------------#


class Server:
    """
    Server object to keep socker open.
    """

    def __init__(self, host: str, port: int):
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        click.echo(f"Open new socket {host}:{port}")
        self.socket.connect((host, port))
        click.echo(f"Connection on {port}")

    def send_answer(self, answer: str) -> str:
        """Send answer then return server answer."""
        click.echo(f"Send answer: {answer}")
        self.socket.sendall(answer.encode("utf-8"))
        return self.receive_data()

    def receive_data(self) -> str:
        """Receive data."""
        click.echo("Call receive data")
        data = self.socket.recv(1024).decode("utf-8")
        click.echo(f"Received '{repr(data)}'")
        return data


# ------------------------- [COMMANDS] -------------------------#


def entry_point(server, _) -> str:
    """First call to starting challenge"""
    incoming_data = server.receive_data()
    click.echo(f"Challenge to solve : '{incoming_data}'")
    return incoming_data


# ------------------------- [BODY] -------------------------#

@click.command(
    context_settings={"help_option_names": ["-h", "--help"]},
    help="Tcp Challenge solver",
)
@click.option("--host", type=click.STRING, help="Domain name.", required=True)
@click.option("--port", type=click.INT, help="TCP port.", required=True)
@click.option(
    "--stop_condition", type=click.STRING, help="String to detect to stop running"
)
def start_challenge(host: str, port: int, stop_condition: Optional[str]):
    """Starting point to solve challenge."""
    click.clear()

    click.echo(f"Connect to server {host}:{port}")
    server = Server(host, port)

    # Define list of command
    commands = [
        partial(entry_point, server),
        # List of command like 
        # partial(fct, server),
    ]

    # Receive challenge
    previous_answer = ""
    flag = None
    for fct in commands:
        previous_answer = fct(previous_answer)
        if stop_condition and stop_condition in previous_answer:
            flag = previous_answer
            break

    if not flag:
        flag = previous_answer
    click.echo(f"[🏆] '{flag}'")


if __name__ == "__main__":
    start_challenge()  # pylint: disable=no-value-for-parameter
```

## Here are some snippet command

- Use regular expression and math

```python
import re
import math

def use_regexp_math(server, input_data: str) -> str:
    """Use RegExp and math"""
    click.echo(f"Challenge to solve : '{input_data}'")

    # Get number
    regexp = re.compile(
        "Number to sqrt ([0-9]*) and multiple by ([0-9]*)"
    )
    match = re.search(regexp, input_data)
    to_sqrt_nbr = int(match.group(1))
    multi_nbr = int(match.group(2))

    # Compute
    result = math.sqrt(multi_nbr)
    click.echo(f"Square of {multi_nbr} {result}")
    result *= multi_nbr
    click.echo(f"Result {result}")
    result = round(result, 2)
    click.echo(f"Round result {result}")

    # Send result
    return server.send_answer(f"{str(result)}\n")
```

- Use base64 and zlib


```python
import base64
import zlib

def ub64_and_zlib_decompress(server, input_data: str) -> str:
    """Decompress after debase 64 strings"""
    msg = input_data.split("'")[1]
    click.echo(f"Message to decode => '{msg}'")
    result = zlib.decompress(base64.b64decode(msg))
    answer = server.send_answer(f"{str(result)}\n")
    click.echo(f"Answer => '{answer}'")

    return answer
```
- Use split

```python
def split_and_collect(server, input_data):
    """Split part of input the format."""
    click.echo("Use split")

    nbrs = input_data[1:-2].split(",")
    order_nbrs = sorted(map(int, nbrs))
    orders_input_data = [str(s) for s in order_nbrs]
    res = f"[{','.join(orders_input_data)}]"

    # Result
    click.echo(f"Result '{res}'")
    return server.send_answer(f"{res}")
```