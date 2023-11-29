# Get and Post

Un petit programme pour faire des requetes `GET` et `POST` en python.
Ce script est la solution au CTF `DGHACK 2023` pour le chall de dev `Feed This Dragon`

```python
#! /usr/bin/env python

import sys
from typing import Callable, List, Dict
from uuid import uuid1
from functools import partial
from dataclasses import dataclass
from pydantic import BaseModel
import requests

console_handler = logging.StreamHandler()
console_handler.setLevel(logging.INFO)
file_handler = logging.FileHandler("log.txt")
file_handler.setLevel(logging.DEBUG)
logger = logging.getLogger()
logger.setLevel(logging.DEBUG)
logger.addHandler(console_handler)
logger.addHandler(file_handler)


URL = "http://feedthisdragon4.chall.malicecyber.com/api/v1"

TRAP: List[str] = ["bomb"]
FOOD: List[str] = ["veggy", "candy", "burger", "food"]
BOOS: List[str] = ["lilboo", "midboo", "bigboo"]
LIFE: List[str] = ["life"]
FOX: List[str] = ["fox"]
MONEY: List[str] = ["coin", "gem"]
SECRET: List[str] = ["secret", "nyan"]


@dataclass
class NextPost:
    """Next item to click."""

    item: str = ""
    shop: str = ""


class Achievement(BaseModel):
    """Achievements form."""

    acquired: bool
    color: str
    description: str
    name: str
    slug: str
    uuid: str


class Item(BaseModel):
    """Items data."""

    type: str
    uuid: str
    x: int
    y: int


class Upgrade(BaseModel):
    """Upgrade data."""

    cost: int
    description: str
    level: int
    name: str
    slug: str
    uuid: str


class GameResponse(BaseModel):
    """Game response"""

    achievements: List[Achievement]
    bag: int
    coin: int
    health: int
    hunger: int
    hunger_actual: int
    hunger_needed: int
    items: List[Item]
    level: int
    max_health: int
    upgrades: List[Upgrade]
    uuid: str


def format_header(*, uuid: str, update: str, itemUuid: str = "", shopUuid: str = ""):
    """Format post header."""
    header = {
        # "Content-Type": "application/json; charset=utf-8",
        "Cookie": f"uuid={uuid}",
        "Authorization": "mynotsosecrettoken",
        "Session": uuid,
        "Update": update,
        "ItemUuid": itemUuid,
        "ShopUuid": shopUuid,
    }
    return header


def extract_items(game_map: GameResponse) -> Dict[str, List[str]]:
    """Allow to extract items information."""
    available_items = {}
    for item in game_map.items:
        key = item.type
        available_items.setdefault(key, []).append(f"[{item.x},{item.y}]")
    return available_items


def get_strategy(game_map: GameResponse) -> List[Callable[[GameResponse], NextPost]]:
    """Apply correct strategies."""
    strategies: List[Callable[[GameResponse], NextPost]] = []

    if not game_map.items:
        return strategies

    # Map status
    not_achieved = [i.slug for i in game_map.achievements if not i.acquired]

    # [STRATEGY] Achievement Saw
    # saw => Why are those items so far from the screen
    if "saw" in not_achieved:
        strategies.append(find_item_coordinate)

    # [STRATEGY] Achievement 'Secret'
    if "secret" in not_achieved:
        strategies.append(partial(find_item, SECRET))

    # [STRATEGY] Achievement hurt
    if "hurt" in not_achieved:
        strategies.append(partial(find_item, TRAP))
    else:
        # [STRATEGY] Kill Boos
        strategies.append(partial(find_item, BOOS))

    # [STRATEGY] Health
    if game_map.health <= 2:
        if game_map.coin == 0:
            strategies.append(partial(find_item, MONEY))
        else:
            strategies.append(partial(find_item, LIFE))
            strategies.append(partial(upgrades, "Hard"))

    # [STRATEGY] Achievement Fatter
    if "fatter" in not_achieved or "bigger" in not_achieved:
        strategies.append(partial(find_item, FOOD))
        strategies.append(partial(upgrades, "Feed"))

    # [STRATEGY] Achievement money
    if "gold" in not_achieved:
        strategies.append(partial(find_item, MONEY))
        strategies.append(partial(upgrades, "Greed"))
        if game_map.bag < 10000:
            strategies.append(partial(upgrades, "Bag"))

    logger.debug("strategies =>")
    for idx, strategy in enumerate(strategies):
        logger.debug(f"\t- [{idx}] {strategy}")

    return strategies


def find_item_coordinate(game_map: GameResponse) -> List[NextPost]:
    """Strategy find item."""
    next_list: List[NextPost] = []
    for item in game_map.items:
        if item.x > 100 or item.y > 100:
            return [NextPost(item=item.uuid)]
    return next_list


def find_item(ref_label: List[str], game_map: GameResponse) -> List[NextPost]:
    """Strategy find item."""
    next_list: List[NextPost] = []
    for item in game_map.items:
        if item.type in ref_label:
            next_list.append(NextPost(item=item.uuid))

    if not next_list:
        logger.debug(f"\tNothing like {ref_label}.")
    return next_list


def upgrades(ref_label: str, game_map: GameResponse) -> List[NextPost]:
    """Strategy Upgrade."""
    for item in game_map.upgrades:
        if item.name == ref_label:
            return [NextPost(shop=item.uuid)]
        logger.debug("\tNothing like %s", ref_label)
    return []


def get_map_info(cur_game: GameResponse):
    """Display map info."""
    logger.info("\n")
    logger.info("-" * 20)
    logger.info(f"Level: {cur_game.level}")
    logger.info(f"Health: {cur_game.health}")
    logger.info(f"Hunger: {cur_game.hunger}")
    logger.info(f"Hunger actual: {cur_game.hunger_actual}")
    logger.info(f"Hunger needed: {cur_game.hunger_needed}")
    logger.info(f"bag => {cur_game.bag}")
    logger.info(f"coin => {cur_game.coin}")

    available_items = extract_items(game_map=cur_game)
    formatted = "| ".join(
        [f"{k} ({len(v)}): {','.join(v)}" for k, v in available_items.items()]
    )
    logger.debug(f"Items: {formatted}")

    # logger.debug(f"Upgrades: {cur_game.upgrades}")

    achievements: List[str] = [
        f"{achievement.slug}"
        for achievement in cur_game.achievements
        if not achievement.acquired
    ]
    logger.info(f"Achievements: {'| '.join(achievements)}")
    return len(achievements) == 0


def get_request(*, uuid: str, update: str) -> List[NextPost]:
    """Get request to send."""
    # Send POST request with JSON data using the json parameter
    response = requests.get(URL, headers=format_header(uuid=uuid, update=update))

    if response.status_code != 200:
        logger.error("Error invalid request")

    # Print the response
    cur_game = None
    try:
        cur_game = GameResponse.model_validate(response.json())
        logger.debug(response.json())
    except:
        logger.error("!!!!!!!!!!!!!! Crash !!!!!!!!!!!!!!!")
        logger.error(response.json())
        sys.exit(1)

    # Display map
    done = get_map_info(cur_game)
    if done:
        logger.info("<<<<<<<<<<<<<<<<<<<<<<< DONE >>>>>>>>>>>>>>>>>>>>>>>>>")
        return []

    # Next pos list
    next_list: List[NextPost] = []

    # done if dead
    if cur_game.health <= 0:
        logger.error("!!! You dead !!!")
        return sys.exit(1)

    strategies = get_strategy(cur_game)
    for idx, fct in enumerate(strategies):
        logger.debug(f"=> Apply {fct}")
        if next_pos := fct(cur_game):
            logger.debug(f"\tSuccess {idx}")
            next_list += next_pos
        else:
            logger.debug(f"\tFailed {idx}")

    return next_list


def post_request(*, uuid: str, itemUuid: str, shopUuid: str):
    """Post request."""
    # Send POST request with JSON data using the json parameter
    response = requests.post(
        URL,
        headers=format_header(
            uuid=uuid, update="true", itemUuid=itemUuid, shopUuid=shopUuid
        ),
    )
    logger.debug(f"Post Answer => {response.json()}")


if __name__ == "__main__":
    NEW_GAME_UUID = str(uuid1())

    # Init party
    logger.info("[Initialize new gameStart new game]")
    _ = get_request(uuid=NEW_GAME_UUID, update="false")

    logger.info("[Start challenge]")
    while True:
        next_post_list = get_request(uuid=NEW_GAME_UUID, update="true")
        if not next_post_list:
            continue

        # _ = input("Post request\n")

        logger.debug(f"Post Items => {next_post_list}")
        for pos in next_post_list:
            post_request(uuid=NEW_GAME_UUID, itemUuid=pos.item, shopUuid=pos.shop)

```