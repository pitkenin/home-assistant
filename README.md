# home-assistant

Documentation of the smart home project I've been building with Home Assistant.

I've been working on this since early 2026. It now includes automatic lighting,
room audio, shopping lists, reminders, plant care and voice control. Some of the
controls are standard Home Assistant features, and others have their own menus.

This is the Hub, where most of the everyday controls are:

![The Hub dashboard with music, weather, lighting and household shortcuts](images/hub-dashboard.webp)

There are more screenshots in the [interface gallery](docs/interface.md).
For the technical details, there's a [system overview](docs/how-it-works.md),
[notes on three of the builds](docs/selected-builds.md) and
[testing notes](docs/testing.md).

## Welcome to the Jungle

Our bathroom is called Jungle. There are vines on the walls, birds on the shelves
and a monkey holding a lightbulb. The lighting and sounds come on automatically
when someone enters. The sounds change with the time of day: birds during the
day, crickets at night, and an occasional hidden melody.

A Raspberry Pi handles the audio through two Squeezelite players. Using two
players lets the sounds fade into each other. There are also separate rules for
shower visits and for playing shared music during a sauna session.

![The Jungle bathroom](images/welcome-to-the-jungle.jpg)

## Lighting

Motion controls much of the lighting, but the wall buttons can override it.
A manual choice can keep a light on or off until the override expires or resets
the next morning. That way, switching a light off doesn't just mean waiting for
the next motion event to turn it back on.

The rules use the time of day and the selected Vibe Mode. Lighting Studio is the
menu for adjusting individual lights, their automatic behaviour and the buttons
that control them. It also shows which lights currently have a manual override.

## Brain's Virtual Memory Extension

The shopping menu has categories and shortcuts for common products. When I get
near a grocery store, my phone shows the shopping list. Laundry notifications
use a smart plug to detect when the washing machine has finished.

There are also reminders for plant checks and repeating household tasks. These
can stay pending until they're done, be snoozed, or be shared between both people
living here. Completing a shared reminder on one phone clears it from the other
as well.

![An example of the phone notification setup](images/brain-s-virtual-memory-extension.jpg)

## Music

Music Assistant handles playback, with room choices for the living room, balcony
and Jungle. The music menu has room controls, genre shortcuts and search. The
automatic rules depend on the room and time of day, but pressing Play can still
start music after automatic playback has stopped for the night.

The living-room voice device has separate Music and Assistant volume controls.
Music can be muted while spoken replies remain audible. This needed a change
to the device firmware; the [audio notes](docs/selected-builds.md#separate-music-and-assistant-volume)
explain how it works.

## Voice control in Finnish

The living-room voice device uses **Hei Jarvis** as its wake word. The English
wake word didn't respond well to my Finnish pronunciation. After trying custom
models, I recorded my own voice and used those recordings to improve recognition.
The current version has been working well for me in the room. The stock wake
word is still available as a fallback.

There's also Habibi, an Android assistant project connected to Home Assistant.

## Maintenance

Home Assistant monitors the Jungle audio players and can try to recover them
when they disconnect. Backups and storage need attention too: a recent review
found old development rollback copies filling the hub and blocking backups.
Those archives have been moved off the hub, and there are now earlier warnings
for low disk space, failed backups and old backups.

The [testing notes](docs/testing.md) cover the checks made so far and the parts
that still need testing.

## About the repository

Home Assistant Green is the main controller, with Zigbee2MQTT and a mix of Wi-Fi,
Zigbee and Matter devices. The Jungle audio Pi and living-room Voice Preview
Edition handle the room audio and voice functions.

The published [automation YAML](automations.yaml) is a snapshot for reference.
The setup also uses scripts, packages, custom integrations and private settings,
so that file alone won't recreate it. Configuration exports are kept separately
from these pages; a routine config change doesn't rewrite the project text.
