# Token from movie

## Mise en place des dépendances

```bash
$ sudo apt update
$ sudo apt install tesseract-ocr
$ sudo apt install libtesseract-dev
$ sudo apt install tesseract-ocr-eng
$ pip install Pillow
$ pip install pytesseract
$ pip install wand
$ pip install opencv-python
```

Extraction d'un token à partir d'une video

```python
#! /usr/bin/env python3

import re
import sys

from PIL import Image
import pytesseract
from wand.image import Image as Img
from pathlib import Path

import cv2

import click

# Pattern to be find
PATTERN = re.compile(r"([0-9]{4})", re.IGNORECASE)

def rm_tree(pth: Path):
    """Remove recursive of folder."""
    if not pth.exists():
        return
    for child in pth.glob("*"):
        if child.is_file():
            child.unlink()
        else:
            rm_tree(child)
    pth.rmdir()


def setup(movie_path: Path):
    """Clean temporary folder"""
    rm_tree(movie_path)
    movie_path.mkdir()


def files(movie_path: Path, working_area: Path) -> cv2.VideoCapture:
    """Read video."""
    setup(working_area)

    # specify the source video path
    src_vid = cv2.VideoCapture(str(movie_path))
    return src_vid

def process(src_video: cv2.VideoCapture, rate, working_area: Path):
    """Process video."""
    # Check if file is open
    if not src_video.isOpened():
        print("[!] Video stream is closed")
        sys.exit(1)

    # Integer name file
    index = 0
    while src_video.isOpened():
        ret, frame = src_video.read()
        if not ret:
            break

        # name each frame and save s png
        name = Path(working_area) / Path(f"frame{index}.png")
        # 
        # Save every rate th
        if not (index % rate):
            cv2.imwrite(str(name.absolute()), frame)
        index += 1

        if cv2.waitKey(10) & 0xFF == ord('q'):
            break
    src_video.release()
    cv2.destroyAllWindows()

def get_token(working_area: Path) -> str:
    """Looking for token."""
    for pic in Path(working_area).glob("*.png"):
        cur_pic = Image.open(str(pic.absolute()))
        text = pytesseract.image_to_string(cur_pic, lang='eng')
        
        if "code" in text:
            text = text.replace("\n", "")
            res = PATTERN.findall(text)[0]
            return f"{res}"

    return ""

@click.command(
    context_settings={"help_option_names": ["-h", "--help"]},
    help="Extract token from pictures.",
)
@click.argument(
    "movie_path",
    type=click.Path(exists=True, file_okay=True, dir_okay=False),
    metavar="MOVIE_PATH",
)
@click.option(
    "--rate", 
    default=100, 
    help="Rate of picture to extract text.",
    show_default=True
)
@click.option(
    "--working_area",
    type=click.Path(exists=False, file_okay=False, dir_okay=True),
    default="image_frames", 
    help="Temporary folder.",
    show_default=True
)
def find_token(movie_path: str, rate: int, working_area: str):
    working_area = Path(working_area)

    vid = files(Path(movie_path), working_area)
    process(vid, rate, working_area)
    print("[+] Token for %s => '%s'", movie_path, get_token(working_area))

if __name__ == "__main__":
    find_token()  # pylint: disable=E1120
```

## Utilisation

```bash
> ./script.py --rate 10 ./movie.mp4
Token for ./movie.mp4 => 'XXXX'
```