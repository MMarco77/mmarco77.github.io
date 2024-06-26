# Get and Post

Simple petit script pour résoudre un challenge mis à dispo. par un server.


```python
#!/usr/bin/env python3
import re
import socket
from base64 import b64encode
import logging

HOST = <SERVER_IP_DOMAIN>
PORT = <SERVER_PORT>

log = logging.getLogger()


class Server:
    """Server object to keep socker open."""
    def __init__(self, host: str, port: int):
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        log.info("Open new socket '%s:%d'", host, port)
        self.socket.connect((host, port))
        log.debug("Connection on %d", port)

    def send_answer(self, answer: str) -> str:
        """Send answer then return server answer."""
        log.debug("Send answer: {answer}")
        self.socket.sendall(answer.encode("utf-8"))
        return self.receive_data()

    def receive_data(self) -> str:
        """Receive data."""
        log.debug("Call receive data")
        data = self.socket.recv(1024).decode("utf-8")
        log.debug("Received '%s'", repr(data))
        return data


def chall_one(input_data, server):
    """Soluce challenge 1: compute answer operation."""
    log.info("Solve: '%s'", input_data)

    # Regular expression
    regex = r"How much is ([0-9]*)\s*\+\s*([0-9]*)\s*\?"

    # Receive first data
    matches = re.findall(regex, input_data)

    # Result
    left = int(matches[0][0])
    right = int(matches[0][1])
    res = left+right
    log.info("Result '%d'", res)
    return server.send_answer(f"{res}")


def chall_two(input_data, server):
    """Soluce challenge 2: Compute base64."""
    log.info("Solve: '%s'", input_data)

    # Regular expression
    regex = r"the base 64 of \"(\w*)\"\s*\?"

    # Receive first data
    matches = re.findall(regex, input_data)

    # Result
    word = matches[0][0]
    res = b64encode(word.encode())
    log.info("Result '%s'", res)
    return server.send_answer(f"{res}")


def chall_three(input_data, server):
    """Soluce challenge 3: sort array."""
    log.info("Solve: '%s'", input_data)

    nbrs = input_data[1:-2].split(",")
    order_nbrs = sorted(map(int, nbrs))
    orders_input_data = [str(s) for s in order_nbrs]
    res = f"[{','.join(orders_input_data)}]"

    # Result
    log.info("Result '%s'", res)
    return server.send_answer(f"{res}")


def start_challenge():
    """Challenges sheduler."""
    server = Server(HOST, PORT)

    next_challenge = server.receive_data()

    log.info("Solve chall 1 => '%s'", next_challenge)
    next_challenge = chall_one(next_challenge, server)

    log.info("Solve chall 2 => '%s'", next_challenge)
    next_challenge = chall_two(next_challenge, server)

    log.info("Solve chall 3 => '%s'", next_challenge)
    flag = chall_three(next_challenge, server)

    log.info("[🏆] '%s'", flag)


if __name__ == "__main__":
    start_challenge()

```