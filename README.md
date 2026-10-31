# home-assistant

Documentation of the smart home project I've been building with Home Assistant.

I started this in early 2026. There's a more technical look at the setup at the
end of this page, with notes on how some of the more involved parts work.

But first, here's the interesting part:

## Welcome to the Jungle

Walking into the bathroom gives you an adventurous transportation into the
Jungle: first thing you notice is the vine-covered interior, birds on the shelves
and a monkey holding a lightbulb, which lights up in a warm color as you walk in.
After a second, you notice the sounds of the jungle begin surrounding you from
all directions, emanating from the speakers located around the bathroom ceiling,
connected to a Raspberry Pi and controlled by Home Assistant's Music Assistant.
As you visit the Jungle more often, you will notice the soundtrack changing
throughout the day, with crickets at night and birds chirping during the day.
On some rare occasions you can even hear a fun little melody playing in the
Jungle!

The Jungle has grown a bit since then. Stay for a shower and you might hear rain
and thunder, with the monkey's light providing the lightning. During a sauna
session, the room can join the music playing in the living room, or keep its own
jungle ambience. And when you step up to the sink to wash your hands, the mirror
cabinet light comes on without you reaching for a switch.

![The Jungle bathroom](images/welcome-to-the-jungle.jpg)

## Brain's Virtual Memory Extension

I couldn't stop at only automating the home and its devices; I also had to
automate my daily life, even outside of home. Since I always carry a phone with
me, I decided to utilize it to the fullest and set up automated notifications so
I would never forget anything! Whenever my phone enters a grocery store's
vicinity, I get a notification listing all items on my "I'll get it next time I
go grocery shopping"-list, if there are any.

Adding things to that list now has its own menu, with pictures of everyday
products. When I notice something running out, I can tap it onto the list and
leave the remembering to my phone. The same idea works at home: the washing
machine's smart plug notices when a load has finished, and the printer's low-ink
notification keeps coming back until I actually refill it.

The plants have joined in too. Each has its own place in the plant menu and a
reminder for when it's time to check it again. If I'm busy, I can snooze the
reminder; if one of us has already taken care of a shared task, completing it
clears it from both phones. Repeating household tasks work this way as well.
The whole idea is to externalize my memory so I can live a little more care-free,
without having to keep a second list of things I still need to remember to do.

![An example of the phone notification setup](images/brain-s-virtual-memory-extension.jpg)

## Automatic lights, until you say otherwise

Walking through the hallway or into the bathroom is enough to turn the lights
on, and at night they come on softer and warmer. That's nice until you decide
you want a light off and the next motion event turns it straight back on again.
I still want an ordinary wall button to do what I asked it to do.

So the button press also tells the room to leave automatic control alone for a
while. I can keep the light off, leave it on, or choose a different brightness.
The button can give me a pause until later or the next morning, after which the
room returns to automatic without me having to remember to switch it back.

There is now a Lighting Studio for adjusting these rules. I can open a light's
card, change how it should behave at different times, and set what its physical
buttons do. The current manual overrides are visible there too, so I can see
why a light is doing what it's doing.

## Time of Day and Vibe Modes

The same room needs different settings depending on what we're doing. A trip to
the toilet at night doesn't need daytime lighting, and cleaning the Jungle is
easier when the lights stay on and the soundtrack stays quiet.

Time of Day moves through Morning, Day, Evening and Night. Vibe Mode lets me
choose an atmosphere across the home, and usually takes priority over the time
of day. Maintenance keeps the lights on and sounds off until I switch it back;
Movie keeps the areas near the living room darker while someone's watching.
These are shared choices, so the rooms can follow the same mode without me
going around changing them individually.

## Music around the home

When there's music playing in the living room, I can open the music menu and
add the balcony to the same session. During a sauna session, Sauna Sync brings
the Jungle in too. The genre shortcuts and search are there as well, so I can
choose what to listen to from the same place. Ordinary automatic playback stops
for the night, but pressing Play still means I want to listen now.

One less obvious problem was what happened when I turned the music all the way
down. The living-room voice device shared that volume with the assistant, so
the next spoken reply could disappear too. It now has separate Music and
Assistant sliders: I can have silence from the music and still hear the answer.
That took a change to the device firmware, which is described in the
[audio notes](docs/selected-builds.md#separate-music-and-assistant-volume).

The Hub brings the everyday controls together. There are more screenshots of
the shopping, plant, lighting and music menus in the
[interface gallery](docs/interface.md).

![The Hub dashboard with music, weather, lighting and household shortcuts](images/hub-dashboard.webp)

## Hei Jarvis

In the living room, I can get the voice device's attention with **Hei Jarvis**.
Getting it to recognise how I actually say that took some experimenting. The
English wake word didn't respond well to my Finnish pronunciation, and the
first custom models still needed too much repeating when I tried them in the room.

So I recorded my own pronunciation from different places in the room and used
those recordings to refine the model. The current version has been working well
for me, and the stock wake word is still there as a fallback. The
[wake-word notes](docs/selected-builds.md#finnish-wake-word) include the small
offline comparison and the room test. I'm also working on Habibi, an Android
assistant connected to the home.

## When something breaks

The bigger this setup gets, the more I want to rely on automatic diagnostics
and repairs. If the Jungle audio players drop off the network, Home Assistant
notices, tells me and can try to recover them. There's also a watchdog on the Pi
itself that can reboot and log independently if HA can't reach it.

Old rollback copies recently filled the hub's storage and blocked backups, so
those archives have been moved off the hub and there are now earlier warnings for disk space,
failed backups and backups getting too old. The aim is to get closer to a
set-and-forget home while keeping track of the parts that still need attention.

## A closer look

Home Assistant Green is the main controller, with Zigbee2MQTT and a mix of Wi-Fi,
Zigbee and Matter devices. The Jungle audio Pi and living-room Voice Preview
Edition handle the room audio and voice functions.

The [system overview](docs/how-it-works.md) shows how the main parts connect.
The [technical notes](docs/selected-builds.md) cover shared reminders, separate
audio levels and the Finnish wake word, and the [testing notes](docs/testing.md)
explain what's been checked and what still needs work.

The published [automation YAML](automations.yaml) is a snapshot for reference.
Scripts, packages, custom integrations and private settings also make up the
setup, so that file alone won't recreate it. These pages are updated separately
from the configuration exports.
