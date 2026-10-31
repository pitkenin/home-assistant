# Three builds in more detail

[Back to the home](../README.md)

## A reminder that is still the same reminder tomorrow

The simple version sends a notification. The harder version knows whether a
button belongs to the reminder that is pending now or to an older one that has
already been completed.

I separated the task from each occurrence of it. A task can repeat, while the
notification and its completion button belong to a particular occurrence.
Before accepting a phone action, the manager checks the sender and that current
occurrence. An old Done button should not complete the next reminder.

For shared tasks, both phones refer to the same completion record. Completing
one copy clears the other. Plant checks add their own scheduling: Done means
checked, and the next check is calculated from that date. It does not mean
that every plant must have been watered.

The records use HA's native to-do storage rather than a separate database for
each feature. That is convenient for persistence and editing, but it still needs
careful handling around retries, old actions and restarts. Those cases have been
checked separately from the real-phone tests.

## Giving music and speech different volume controls

The [music control screenshot](interface.md#music) shows the two levels in use.

The original living-room setup exposed one physical audio level. Fading music
to zero could also make assistant speech and wake sounds inaudible. Two sliders
would not solve that if both still changed the same output.

The custom firmware now adjusts music and announcement audio separately before
they reach the shared output. In implementation terms, each source has a
software gain applied to its PCM samples. The hardware output stays at a fixed
ceiling, and short ramps smooth changes in gain.

The change retains the existing mixing and playback timing. It also follows
the old volume curve so familiar music levels do not suddenly become much louder.
The physical speaker knob still affects both paths; the separation is within
the voice device, not two independent amplifiers.

Checks covered the actual gain code, independent mute and volume, and positive
and negative full-scale samples. On the installed device, changing Assistant
left Music alone, and music at zero left the assistant level intact. Audible
checks of every interruption path are a separate task, not something the numeric
checks prove.

## Making a wake word fit the person using it

The first Finnish wake-word models looked promising in offline tests but were
unreliable in the room. Changing sensitivity did not fix that. The useful next
step was a recording of my own pronunciation, including speaking from different
places in the room.

The personal recording was divided into training, validation and test portions
before training. The new model was also checked with background audio for false
activations. On six reserved phrases from that recording session, the previous
model detected two and the new one detected all six at their respective tested
settings.

That is a small comparison from one speaker and one recording session. It is
evidence for an improvement, not a general Finnish recognition score. The more
useful follow-up was trying it on the installed device: I confirmed that it was
working well in the room. The fallback wake word and the previous firmware
remain available for recovery.

Across these builds, the recurring lesson has been to separate a feature into
parts that can be checked independently, then still test the part a person
actually experiences.
