# What I can reasonably claim

[Back to the home](../README.md)

This is a home I use and keep changing. The checks below are different kinds
of evidence, and I don't treat them as interchangeable.

| Check | What it tells me | What it does not prove |
| --- | --- | --- |
| HA configuration validation | The edited configuration can load | That the room behaves as intended |
| Rule and template cases | Selected inputs lead to the expected decision | Every real device or timing combination |
| Phone notification tests | Delivery and buttons worked in tested cases | Every Android version or network state |
| Firmware gain tests | The actual gain calculation preserves independent levels | How the speakers sound in the room |
| Offline wake-word tests | Performance on the recorded examples and background set | Recognition for other people or all room conditions |
| Using the installed feature | It worked in the observed situation | That it cannot regress later |

## Examples of checks already made

The reminder work has included shared completion, stale buttons, retries,
edits and reloads. Plant checks have also been completed from a real phone.
The independent audio change has native gain-code tests and installed-device
volume checks. The personal Finnish wake-word revision has both offline results
and a positive room test from me.

Music routing has been checked for room changes, pauses and several recovery
cases. Some physical controls and interruption paths still need end-to-end
listening tests. In particular, a successful volume-restore template test is
not the same thing as hearing balcony audio duck and recover during a doorbell.

## What still needs care

Hardware, network availability and third-party services can all change the
result. The custom voice firmware needs its changes carried forward when
updating; installing stock firmware would not preserve them automatically.

Storage was another reminder of that. Development rollback copies accumulated
on the hub until backups stopped succeeding. The response was to move verified
old archives off the hub, reduce local automatic-backup retention, and add earlier
disk and backup-age warnings. Those measures reduce the chance of a repeat;
they are not a reason to stop checking backups.

I keep the distinction between a test fixture, a live test and something I
have confirmed in everyday use in the development notes. The public pages are
a shorter account of those notes, rather than a claim that everything is finished.
