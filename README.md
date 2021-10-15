![Lenovo Ideapad / Legion 5 Pro 2021 Linux RGB Keyboard Light Controller](https://i.imgur.com/FhBMS9W.jpg)

# Lenovo Ideapad Legion 5 Pro 2021 Linux RGB Keyboard Light Controller

This util allows to drive RGB keyboard light on Lenovo Ideapad or Legion 5 Pro 2021 Laptop

## Requirements

* pyusb
* changing VENDOR and PRODUCT with yours ( google "find vendor id lsusb" )

## Install

### Regular python
```
git clone https://github.com/InstinctEx/lenovo-ideapad-legion-keyboard-led
cd lenovo-ideapad-legion-keyboard-led
python -m venv env
source ./env/bin/activate
pip install -r requirements.txt
python lenovolight.py --help
```

### Archlinux
```
sudo pacman -Sy python-pyusb
git clone https://github.com/InstinctEx/lenovo-ideapad-legion-keyboard-led
cd lenovo-ideapad-legion-keyboard-led
python lenovolight.py --help
```

### Unprivileged usage

Add udev rule if you want to switch light as unprivileged user
```
# /etc/udev/rules.d/10-kblight.rules
SUBSYSTEM=="usb", ATTR{idVendor}=="048d", ATTR{idProduct}=="c963", MODE="0666"
```

Reload rules
```
sudo udevadm control --reload-rules && sudo udevadm trigger
```

## Usage

### Colors

I'ts possible to set color for 4 sections of keyboard separately like `COLOR COLOR COLOR COLOR`, but you can set single color for whole keyboard with just `COLOR`. Last color will be repeated for other sections.

#### HEX
HEX colors should be 6-digit base 16 input like `ffffff`

#### RGB
RGB colors should be 0-255 decimal values for each component, separated by comma like `255,0,127`

#### HSV
HSV colors should be 0.0-1.0 float values for hue, saturation, value, separated by comma like `0.0,1.0,0.25`

### Light effect

#### statiс
Static color

```
usage: lenovolight.py static [-h] [--brightness {1,2}] colors [colors ...]

positional arguments:
  colors              Colors of sections

optional arguments:
  -h, --help          show this help message and exit
  --brightness {1,2}  Light brightness
```

Turn 100% red
```sh
./lenovolight.py static ff0000
```

At full brightness, turn 100% red for 1 section, 100% green for 2 section, 100% blue for 3 section an 100% white for 4 section
```sh
./lenovolight.py static ff0000 00ff00 0000ff ffffff --brightness 2
```

Dimmed warm orange like wire heater. With HSV color input.
```sh
./lenovolight.py static 0.02,1,0.2
```

#### breath
Fade light in and out
```
usage: lenovolight.py breath [-h] [--brightness {1,2}] [--speed {1,2,3,4}] colors [colors ...]

positional arguments:
  colors              Colors of sections

optional arguments:
  -h, --help          show this help message and exit
  --brightness {1,2}  Light brightness
  --speed {1,2,3,4}   Animation speed
```

Fast white blink at full brightness
```sh
./lenovolight.py breath ffffff --speed 4 --brightness 2
```

#### hue
Transition across hue circle. You can't set custom colors here.
```
usage: lenovolight.py hue [-h] [--brightness {1,2}] [--speed {1,2,3,4}]

optional arguments:
  -h, --help          show this help message and exit
  --brightness {1,2}  Light brightness
  --speed {1,2,3,4}   Animation speed
```

Rotate HUE slowly
```sh
./lenovolight.py hue --speed 1
```


#### wave
Rainbow wawe effect. Wow. Cool. Useles.
```
usage: lenovolight.py wave [-h] [--brightness {1,2}] [--speed {1,2,3,4}] {ltr,rtl}

positional arguments:
  {ltr,rtl}           Direction of wave

optional arguments:
  -h, --help          show this help message and exit
  --brightness {1,2}  Light brightness
  --speed {1,2,3,4}   Animation speed
```

Wheeee, left to right
```sh
./lenovolight.py wave ltr
```

Pew-pew-pew, right to left
```sh
./lenovolight.py wave rtl --speed 4
```

#### off
Turn light off. Nuff said.
```
usage: lenovolight.py off [-h]

optional arguments:
  -h, --help  show this help message and exit
```


## Recommendations
Set `Super+Space` keystroke to turn light on and turn it off with single `fn+Space` press

## Payload description
Device vendor = 048d, product = c963

```
HEADER ........... cc
HEADER ........... 16
EFFECT ........... 01 - static / 03 - breath / 04 - wave / 06 - hue
SPEED ............ 01 / 02 / 03 / 04
BRIGHTNESS ....... 01 / 02
RED SECTION 1 .... 00-ff
GREEN SECTION 1 .. 00-ff
BLUE SECTION 1 ... 00-ff
RED SECTION 2 .... 00-ff
GREEN SECTION 2 .. 00-ff
BLUE SECTION 2 ... 00-ff
RED SECTION 3 .... 00-ff
GREEN SECTION 3 .. 00-ff
BLUE SECTION 3 ... 00-ff
RED SECTION 4 .... 00-ff
GREEN SECTION 4 .. 00-ff
BLUE SECTION 4 ... 00-ff
UNUSED ........... 00
WAVE MODE RTL .... 00 / 01
WAVE MODE LTR .... 00 / 01
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
UNUSED ........... 00
```
