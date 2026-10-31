# Testing and remaining work

[Back to the project](../README.md)

The setup is still being developed. Configuration checks, code tests and tests
with the actual devices cover different parts of it.

## Checks so far

| Area | Checks made |
| --- | --- |
| Configuration | HA configuration validation and selected rule/template cases |
| Reminders | Shared completion, stale buttons, retries, edits and reloads |
| Plant checks | Completion from a real phone |
| Audio firmware | Tests of the gain code, independent mute/volume and installed-device volume controls |
| Finnish wake word | Reserved recordings, background audio checks and a room test |
| Music routing | Room changes, pauses and several recovery cases |

Configuration validation catches loading problems. Rule and code tests check
particular inputs and outputs. Phone tests and listening tests are still needed
to check the result on the actual devices.

## Remaining checks

Some physical controls and audio interruption paths still need testing from
start to finish. For example, the balcony volume-restore templates have been
checked, but ducking and recovery during a doorbell still need a listening test.
The independent Music and Assistant controls also need listening checks across
all interruption paths.

The wake-word comparison uses a small set of my own recordings. Recognition
for other speakers and different room conditions hasn't been established.

## Maintenance

The custom voice firmware needs its changes carried forward when updating.
Installing stock firmware would remove those changes.

Old development rollback copies recently filled the hub's storage and prevented
backups from completing. The old archives were verified and moved off the hub,
local backup retention was adjusted, and warnings were added for disk space,
backup failures and backup age.

The detailed development notes keep the individual test results. These pages
summarise the current setup and the work still to do.
