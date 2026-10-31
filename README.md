# A home that remembers the small things

I've been building this Home Assistant setup since early 2026. It started with
lights, but a bathroom full of jungle sounds and a phone that remembers the
shopping list were apparently where that was going to lead.

Most of the work is in the less visible details: letting a wall button overrule
an automatic light, making a reminder mean the same thing on two phones, and
getting music back into the right room after something reconnects.

For a closer look, start with [how the pieces fit together](docs/how-it-works.md),
[three builds in more detail](docs/selected-builds.md), or
[what I've tested and what still needs work](docs/testing.md).

![The Hub dashboard with music, weather, lighting and household shortcuts](images/hub-dashboard.webp)

The Hub is the everyday control page. The [interface gallery](docs/interface.md)
shows the shopping catalogue, plant checks, lighting editor and music controls.

## Welcome to the Jungle

The bathroom is called Jungle. There are vines, birds on the shelves, and a monkey
holding a lightbulb. Walk in and the light comes on; the speakers around the
ceiling bring in a soundtrack that changes through the day. Birds give way to
crickets at night, with an occasional surprise tucked into the ordinary sounds.

Behind that is a Raspberry Pi running two audio players. Having two lets one
sound fade into another rather than cutting off halfway through. The room has
grown its own sound library and controls, including rules for shower visits and
shared music during a sauna session. The little effect of walking into a room
has ended up being one of the most involved parts of the project.

![The Jungle bathroom](images/welcome-to-the-jungle.jpg)

## Automatic lights, until you say otherwise

I like lights coming on when I need them. I like it less when they turn back on
after I've deliberately switched them off. Physical controls therefore have a
place in the lighting rules, rather than being an afterthought.

The lighting editor brings room rules, manual holds and button behavior into one
place. Time of day and the selected atmosphere give the house a starting point;
a deliberate button press can change what the room is doing. The aim is that
someone can use the lights without needing to understand why HA chose them.

## Brain's virtual memory extension

Some things are more useful remembered outside the house. My phone brings up
the shopping list when I reach a store, and the washing machine's smart plug
provides the clue that a load has finished.

Reminders now go beyond a message that disappears into a notification drawer.
Some stay pending until they're done, can be snoozed, and can be shared between
the two people living here. Plant checks and repeating household tasks use the
same underlying notification system. That also means fixing a problem in how
Done works can help several features at once.

![An example of the phone notification setup](images/brain-s-virtual-memory-extension.jpg)

## Music that knows which room it's in

Music can follow the selected rooms, with separate rules for the balcony and
Jungle. Changing a room or pausing playback should not require rebuilding the
whole session. There are also times when the automatic rules need to give way:
pressing Play at night is a deliberate request, even if ordinary automatic music
has stopped for the evening.

The living-room voice device now has separate music and assistant levels. Turning
music down to zero no longer has to make the next spoken answer disappear with it.
That required changing the device's audio handling, rather than adding another
slider connected to the same volume control.

## Saying it in Finnish

The living-room device listens for **Hei Jarvis**. The English wake word did not
respond well to my Finnish pronunciation, and an early custom model still needed
too many attempts in the room. I recorded my own voice, used it to refine the
model, and tested it again where I actually use it. That version is now working
well for me. The stock wake word remains available as a fallback.

There's also Habibi, an Android assistant project connected to the home. Voice
control has become a project of its own: wake recognition, listening, phone
integration and deciding when to speak are separate problems.

## Keeping it usable

There are recovery checks for things such as the Jungle audio players, but I
wouldn't call the whole home set-and-forget. A recent review found development
rollback files filling storage and blocking backups. Moving old archives off the
hub, checking backup destinations, and adding earlier warnings were more useful
than another clever automation.

That is part of the project too: noticing where an idea creates maintenance,
then making it easier to live with. The [technical notes](docs/selected-builds.md)
cover some of those decisions without trying to document every switch.

## What is in this repository

The pages here are a maintained showcase. They don't rewrite themselves when a
timer or entity reference changes. The published [automation YAML](automations.yaml)
is a selected snapshot, not a complete installer: scripts, packages, integration
setup and private values also matter. This repository is not a backup of the home.

Home Assistant Green coordinates the setup, with Zigbee2MQTT and a mix of Wi-Fi,
Zigbee and Matter devices. The Jungle audio Pi and living-room Voice Preview
Edition handle different audio jobs. More detail is in the
[system overview](docs/how-it-works.md).
