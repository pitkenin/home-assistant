# home-assistant

Documentation of the smart home project I've been building with Home Assistant.

Keeping it written by hand was never going to happen. Every tweak and every new
automation would mean editing a README, leaving me with less time for my projects (including this one). 
So, I automated it. The overview below is generated from my actual automation config and rewritten daily if there were any changes made.
And the best part is: I don't even need to think about it. You can read about the technical details at the end of this file. 

But first, here's the interesting part:

## The Smart Home

<!-- START_AI_SUMMARY -->

This is the home I've spent the last few years quietly rebuilding into something that thinks along with me: a hallway that lights itself, a bathroom that turns into a jungle, notifications that remember things so I don't have to, and a handful of failsafes running in the background so it mostly takes care of itself. Here's a tour through how it actually behaves, room by room and mode by mode.

### Welcome to the Jungle

Walking into the bathroom gives you an adventurous transportation into the Jungle: vines cover the walls, birds sit on the shelves, and a monkey hangs from the ceiling holding a lightbulb that glows warm the moment you step in. A beat later the jungle wakes up around you, sound pouring in from ceiling speakers wired to a Raspberry Pi and orchestrated through Music Assistant. The soundtrack isn't static — it follows the time of day, so late at night the room settles into crickets, while the middle of the day brings birdsong back out. Stay long enough on the rare occasion and you might catch a little melody slip in, a small easter egg that only shows up once in a while. Step up to the mirror to wash your hands and the cabinet light above it switches on by itself, sensing you're right there, and fades back out the moment you step away.

### Brain's Virtual Memory Extension

I couldn't stop at automating the home itself; I wanted my daily life outside it handled too. My phone comes with me everywhere, so it's become the messenger. Walk near a grocery store and it lists everything sitting on the shopping list, so I never stand in an aisle trying to remember what I ran out of. Finish a load of laundry and it tells whoever started it that the machine's gone idle. When the printer's ink runs low it sends a notification that doesn't just go away when swiped — it keeps quietly nagging until I've actually put a new cartridge in. The whole point is to treat my phone as an extension of memory, so I can move through the day a little more care-free and trust that the things I'd otherwise forget will find me anyway.

![Brain's Virtual Memory Extension](images/brain-s-virtual-memory-extension.jpg)

### Automatic lights, until you say otherwise

Motion sensors handle most of the lighting in the hallway and around the house, turning lights on when someone walks in and letting them fade back off once the room's been empty a while. But automatic lighting stops feeling clever the moment it does something you don't want, so every room with motion control also has a manual override sitting right next to it. A single press of the button takes that room off automatic and does exactly what you asked; a double press locks it fully on or off. The important part is what happens next — the override quietly expires on its own after a couple of hours, or resets the next morning, and the room slides back onto automatic without me ever having to remember to switch it back. The automation never fights you; it just steps aside when you want it to, and steps back in later.

### Time of Day and Vibe Modes

Underneath all of it are two modes that almost everything else reads from. Time of Day moves through Morning, Day, Evening, and Night on its own schedule, and it's what decides things like whether the hallway light comes on soft and dim or bright and full, or whether the Jungle is playing birds or crickets. Vibe Mode sits above it and is set by hand for a particular atmosphere, and it usually wins the argument when the two disagree. At night, lighting leans warmer and dimmer across the board. Switch on Maintenance and lights stay lit and sound stays off until I switch it back, useful for cleaning or fixing things without the house trying to help. Switch on Movie and the areas around the living room dim down so nothing spills onto the screen while someone's watching something good.

### When something breaks

The bigger this system gets, the less I want to be the one who notices when something's gone wrong. If the Jungle speakers vanish from the network, Home Assistant catches it after a few minutes of silence, sends me a notification, and tries rebooting the speaker itself before reloading its connection. As a second line of defense there's a watchdog running directly on the Raspberry Pi, independent of Home Assistant entirely, that reboots and logs on its own in case the whole Pi becomes unreachable. The goal isn't to have zero failures, since with enough moving parts something will eventually hiccup, it's to have the house notice and fix itself before I do. Set and forget, as much as a jungle bathroom can be.


<!-- END_AI_SUMMARY -->

## How the automatic documentation works

```mermaid
flowchart LR
    A["automations.yaml"] --> B["Git Exporter"]
    B --> C["private repo"]
    C --> D["GitHub Actions"]
    D --> E["Claude"]
    E --> F["this file"]
```

Once a day, an automation in Home Assistant triggers an add-on that pushes the config to
a private repository. A GitHub Action there passes the automations to Claude, which
groups them into themes and writes the section above. The `description` field on each 
automation is written by hand when I build it, and the model uses it to help it 
determine what the automation does.

The system hashes the automation set before doing anything, so an unchanged config costs no
API calls and produces no commit. It feeds the previous version back as context, so
sections stay where they are instead of being reorganised on every run. And two
deterministic scanners check both the source config and the generated text against
patterns and a private string list before anything is published — if either trips,
the run fails and the README is left alone.



## Hardware

Home Assistant Green serves as the hub, with a Zigbee2MQTT bridge for the Zigbee side.

Around 35 devices in total, across the living room, hallway, toilet, two bedrooms, and
the bathroom (which is called Jungle).

Lighting is mostly WiZ RGB bulbs, with Shelly relays behind fixtures I couldn't
replace. Input comes mostly from IKEA SOMRIG buttons on the walls, Aqara P1 motion sensors,
an Aqara vibration sensor on a door, and a smart plug for appliance
power. A battery IR blaster covers the devices with no network control.

Audio runs on a separate Raspberry Pi Zero 2 W in the bathroom, driving two
Squeezelite instances through a DeLOCK 7.1 USB sound card, Y-split to a pair of
Logitech Z150s and a pair of Trust Polo 2.0s. Music Assistant controls sounds and music.
Elsewhere there's a NVIDIA SHIELD, a Google Nest Mini, a Roborock S5 Max, and an Epson ET-8550.

