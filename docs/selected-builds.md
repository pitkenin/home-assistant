# Technical notes

[Back to the project](../README.md)

These are three parts of the setup that needed more work than a regular HA
automation. Each section explains the problem and the current implementation.

## Shared reminders

A repeating task needs to distinguish today's reminder from the next one.
Otherwise, pressing Done on an old phone notification could complete a newer
reminder by mistake.

The task and its individual occurrences have separate records. Each notification
action belongs to a particular occurrence. Before accepting it, the notification
manager checks the sender and the current occurrence. A button from an older
notification can't complete the next reminder.

Both phones refer to the same completion record for a shared task. Completing
it on one phone clears it from the other. Plant checks use this too, with the
next check calculated from the completion date. Here Done means the plant has
been checked; it doesn't require watering every time.

The records use HA's native to-do storage. Checks have covered stale actions,
retries, edits and reloads, as well as completion from a real phone.

## Separate music and assistant volume

The living-room voice device originally had one output level for music, spoken
replies and wake sounds. Fading music to zero could make the assistant inaudible
too.

The custom firmware applies a separate software gain to each source's audio
samples (PCM). The hardware output stays at a fixed ceiling. Short ramps smooth
volume changes, and the existing volume curve is preserved so familiar settings
don't suddenly become much louder.

This keeps the existing audio mixing and playback timing. The physical speaker
knob still affects both sources; the separate controls are within the voice
device. The [music menu screenshot](interface.md#music) shows music at zero and
the assistant at 26%.

The gain code has tests for independent mute and volume, including positive and
negative full-scale samples. Checks on the installed device confirmed that
changing Assistant left Music alone, and muting Music left the assistant level
intact. Some interruption paths still need listening tests.

## Finnish wake word

Early custom wake-word models performed better in offline tests than in the
room. Adjusting sensitivity wasn't enough. I recorded my own pronunciation from
different places in the room and used it to refine the model.

The recording was split into training, validation and test portions before
training. The model was also checked against background audio for false
activations. On six reserved phrases from that recording session:

| Model | Phrases detected at its tested settings |
| --- | --- |
| Previous version | 2 of 6 |
| Personal revision | 6 of 6 |

That's a small test with one speaker from one recording session. It doesn't
give a general score for Finnish recognition. I've also tried the installed
version and confirmed that it works well for me in the room. The stock wake
word and previous firmware remain available as fallbacks.
