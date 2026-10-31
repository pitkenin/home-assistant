# How I built it

[Back to the project and screenshots](../README.md)

Home Assistant Green coordinates the home, with Zigbee2MQTT for the Zigbee
devices, a Raspberry Pi for Jungle audio and a Voice Preview Edition in the
living room. The custom work sits across HA scripts and packages, dashboard
interfaces and the voice device's firmware.

## The Jungle audio system

The bathroom uses two Squeezelite players on the same Raspberry Pi. Both feed
the same speakers. Having two playback paths lets one recording fade into the
next, and lets a short sound play over the background ambience.

The sound library stores each recording's category, relative loudness, selection
weight and allowed triggers. Ambience has slots for different times of day and
rain. A controller chooses the recordings and manages the players, including
crossfades and how much background sound remains under a melody. The library
editor lets me change individual recordings and their settings.

The controller remembers which surprise-sound categories have already played
during a visit. Each shower gets a separate opportunity for a sound or
thunderstorm. Storms combine rain ambience, thunder recordings and flashes from
the monkey light. Each thunder recording has its own flash count, and the gaps
between strikes gradually lengthen.

Sauna Sync changes the room's role. It can join the living-room music group
while the original ambience mixer is suspended, then return to Jungle ambience
when that choice is switched back.

## Lighting Studio and manual control

I separated who switches a light on and off from how it should look while it's
on. A light can follow motion, or leave power to buttons and other routines
while its brightness and colour follow the time of day. The daylight option
uses the existing Adaptive Lighting curve.

Time of Day and Vibe Mode are shared inputs, with individual looks for each
light. A default look can cover the whole day, with separate settings for
Morning, Day, Evening or Night where needed. Groups can give each bulb its own
look.

Physical controls have their own mappings for taps, double taps, holds and
wheel rotation. Their actions can carry a manual pause for a duration, until
the next visit, or until a chosen time. Automatic lighting respects that pause.
Lighting Studio brings those mappings and light rules into one editor, alongside
the current manual state. The room's active mode stays separate from the mode
being edited, so I can prepare a nighttime look during the day.

## Shared reminders and household menus

Shopping, plant checks and repeating tasks use the home's to-do records and
shared notification tools. The custom shopping menu adds illustrated product
shortcuts to the same list that appears on the phone. The plant menu adds
individual schedules and completion actions.

A repeating task has its own record, and each time it becomes due has a separate
occurrence. Phone actions refer to that occurrence, with the sender and current
record checked before accepting completion. Both phones use one completion
record for a shared reminder, so marking it done on one clears the other copy.

Plant checks calculate the next date from completion. Snooze postpones the
notification while keeping the task pending. Scheduling belongs to the feature;
delivery and the Done/Snooze actions belong to the shared notification manager.
That gives several household menus the same behaviour on the phones.

## Separate music and assistant volume

The living-room voice device originally shared one output level between music
and announcements. I changed its firmware so the music and announcement paths
each have a software gain applied to their audio samples, before reaching the
shared hardware output. Those samples are PCM: the numeric representation of
the sound.

The hardware output stays at a fixed ceiling, with short ramps smoothing changes
in gain. The existing volume curve is preserved so familiar settings keep their
meaning, and the existing mixing and playback timing remain in place.

The Music slider controls the selected music outputs. Assistant controls spoken
replies, wake sounds and living-room announcements independently. That is why
the [music screenshot](../README.md#music-around-the-home) can show music at zero
with the assistant still at 26%.

## A Finnish wake word

I refined the wake-word model with recordings of my own pronunciation, including
speaking from different places in the room. This made the model better suited
to how I say **Hei Jarvis**, after sensitivity changes alone hadn't been enough.

The model runs on the Voice Preview Edition itself. Recognising the phrase
starts the Home Assistant voice interaction. Habibi is the related Android
assistant project connected to the same home.
