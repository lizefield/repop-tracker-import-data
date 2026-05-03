# Repop Import JSON Specification

This document describes the JSON format used for importing data into Repop Tracker.

The JSON should define one or more games and the monsters or events that belong to those games.

## Basic Structure

```json
{
  "version": 1,
  "games": [
    {
      "name": "Example Game"
    }
  ],
  "monsters": [
    {
      "gameName": "Example Game",
      "name": "Example Event",
      "tags": ["event"],
      "respawnType": "fixed",
      "notification": {
        "enabled": true,
        "minutesBefore": [10]
      },
      "isActive": true,
      "fixedRule": {
        "weekdays": [1, 3, 5],
        "time": "21:00",
        "holdSecondsAfterRespawn": 10
      }
    }
  ]
}
```

## Relationship Between `games` and `monsters`

Each monster or event belongs to a game.

The `games[].name` value is the game definition, and each `monsters[].gameName` must match one of those game names.

```json
{
  "games": [{ "name": "Example Game" }],
  "monsters": [
    {
      "gameName": "Example Game",
      "name": "Example Event"
    }
  ]
}
```

When importing:

- If a game with the same name already exists, the existing game can be reused.
- If it does not exist, the app can create it.
- Monsters and events are associated with the game by `gameName`.

## Root Fields

### `version`

Schema version.

```json
"version": 1
```

### `games`

List of games included in the import file.

```json
"games": [
  { "name": "Example Game" }
]
```

### `monsters`

List of monsters, bosses, events, or other respawn targets.

```json
"monsters": []
```

## Game Fields

### `name`

Game name.

```json
"name": "Example Game"
```

This value is referenced by `monsters[].gameName`.

## Monster / Event Fields

### `gameName`

The game name this entry belongs to.

```json
"gameName": "Example Game"
```

This should match one of the values in `games[].name`.

### `name`

Display name of the monster, boss, or event.

```json
"name": "Example Event"
```

### `tags`

Tags used for filtering and grouping.

```json
"tags": ["event"]
```

Multiple tags are allowed.

```json
"tags": ["boss", "field"]
```

### `respawnType`

Respawn type.

```json
"respawnType": "fixed"
```

Allowed values:

| Value     | Meaning                                          |
| --------- | ------------------------------------------------ |
| fixed     | Respawns at fixed weekdays and time              |
| afterKill | Respawns after a specified time from kill record |

### `notification`

Notification settings.

```json
"notification": {
  "enabled": true,
  "minutesBefore": [10]
}
```

Fields:

| Field         | Type     | Description                                    |
| ------------- | -------- | ---------------------------------------------- |
| enabled       | boolean  | Enables or disables notifications              |
| minutesBefore | number[] | Notification timings in minutes before respawn |

### `isActive`

Whether this entry is active.

```json
"isActive": true
```

Inactive entries may be ignored or hidden depending on the app behavior.

## Fixed Respawn Rule

Use `fixedRule` when `respawnType` is `fixed`.

```json
"fixedRule": {
  "weekdays": [1, 3, 5],
  "time": "21:00",
  "holdSecondsAfterRespawn": 10
}
```

### `weekdays`

Weekdays when the target respawns.

| Value | Day       |
| ----: | --------- |
|     0 | Sunday    |
|     1 | Monday    |
|     2 | Tuesday   |
|     3 | Wednesday |
|     4 | Thursday  |
|     5 | Friday    |
|     6 | Saturday  |

Every day:

```json
"weekdays": [0, 1, 2, 3, 4, 5, 6]
```

### `time`

Respawn time in `HH:mm` format.

```json
"time": "21:00"
```

### `holdSecondsAfterRespawn`

Seconds to keep the entry in the current position after respawn.

```json
"holdSecondsAfterRespawn": 10
```

## After-Kill Respawn Rule

Use `afterKillRule` when `respawnType` is `afterKill`.

```json
"afterKillRule": {
  "respawnMinutes": 1200,
  "holdSecondsAfterRespawn": 10
}
```

### `respawnMinutes`

Number of minutes after a kill record until the next respawn.

```json
"respawnMinutes": 1200
```

### `holdSecondsAfterRespawn`

Seconds to keep the entry in the current position after respawn.

```json
"holdSecondsAfterRespawn": 10
```

## Notes

- Use generic, stable names where possible.
- Check event schedules before publishing updates.
- Keep language-specific files separated when names differ by language.
