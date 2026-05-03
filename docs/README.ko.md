# Repop 가져오기 JSON 사양

이 문서는 Repop Tracker에 데이터를 가져오기 위한 JSON 형식을 설명합니다.

JSON에는 게임 정의와 해당 게임에 속한 몬스터, 보스, 이벤트 등의 데이터를 작성합니다.

## 기본 구조

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

## `games`와 `monsters`의 관계

각 몬스터나 이벤트는 하나의 게임에 연결됩니다.

`games[].name`은 게임 정의이며, `monsters[].gameName`은 해당 게임 이름과 일치해야 합니다.

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

가져오기 시 동작:

- 같은 이름의 게임이 이미 있으면 기존 게임을 사용할 수 있습니다.
- 없으면 새 게임으로 생성될 수 있습니다.
- 몬스터와 이벤트는 `gameName`을 통해 게임에 연결됩니다.

## 루트 필드

### `version`

JSON 사양 버전입니다.

```json
"version": 1
```

### `games`

가져오기 대상 게임 목록입니다.

```json
"games": [
  { "name": "Example Game" }
]
```

### `monsters`

가져오기 대상 몬스터, 보스, 이벤트 등의 목록입니다.

```json
"monsters": []
```

## 게임 필드

### `name`

게임 이름입니다.

```json
"name": "Example Game"
```

이 값은 `monsters[].gameName`에서 참조됩니다.

## 몬스터 / 이벤트 필드

### `gameName`

이 항목이 속한 게임 이름입니다.

```json
"gameName": "Example Game"
```

`games[].name` 중 하나와 일치해야 합니다.

### `name`

몬스터, 보스, 이벤트 등의 표시 이름입니다.

```json
"name": "Example Event"
```

### `tags`

필터링과 분류에 사용하는 태그입니다.

```json
"tags": ["event"]
```

여러 개를 지정할 수 있습니다.

```json
"tags": ["boss", "field"]
```

### `respawnType`

리스폰 유형입니다.

```json
"respawnType": "fixed"
```

허용 값:

| 값        | 의미                             |
| --------- | -------------------------------- |
| fixed     | 지정된 요일과 시간에 리스폰      |
| afterKill | 처치 기록 후 지정 시간 뒤 리스폰 |

### `notification`

알림 설정입니다.

```json
"notification": {
  "enabled": true,
  "minutesBefore": [10]
}
```

필드:

| 필드          | 타입     | 설명                            |
| ------------- | -------- | ------------------------------- |
| enabled       | boolean  | 알림 활성화 여부                |
| minutesBefore | number[] | 리스폰 몇 분 전에 알림을 보낼지 |

### `isActive`

이 항목을 활성화할지 여부입니다.

```json
"isActive": true
```

비활성 데이터의 처리는 앱 구현에 따라 달라질 수 있습니다.

## 고정 리스폰 설정

`respawnType`이 `fixed`인 경우 `fixedRule`을 지정합니다.

```json
"fixedRule": {
  "weekdays": [1, 3, 5],
  "time": "21:00",
  "holdSecondsAfterRespawn": 10
}
```

### `weekdays`

리스폰되는 요일입니다.

|  값 | 요일   |
| --: | ------ |
|   0 | 일요일 |
|   1 | 월요일 |
|   2 | 화요일 |
|   3 | 수요일 |
|   4 | 목요일 |
|   5 | 금요일 |
|   6 | 토요일 |

매일:

```json
"weekdays": [0, 1, 2, 3, 4, 5, 6]
```

### `time`

리스폰 시간입니다. `HH:mm` 형식으로 지정합니다.

```json
"time": "21:00"
```

### `holdSecondsAfterRespawn`

리스폰 후 현재 위치를 유지할 초 단위 시간입니다.

```json
"holdSecondsAfterRespawn": 10
```

## 처치 후 리스폰 설정

`respawnType`이 `afterKill`인 경우 `afterKillRule`을 지정합니다.

```json
"afterKillRule": {
  "respawnMinutes": 1200,
  "holdSecondsAfterRespawn": 10
}
```

### `respawnMinutes`

처치 기록 후 몇 분 뒤에 리스폰되는지입니다.

```json
"respawnMinutes": 1200
```

### `holdSecondsAfterRespawn`

리스폰 후 현재 위치를 유지할 초 단위 시간입니다.

```json
"holdSecondsAfterRespawn": 10
```

## 참고

- 게임명과 이벤트명은 언어별로 다를 수 있으므로 언어별 JSON으로 관리합니다.
- 공개 전에 게임 내 일정이나 공식 공지를 확인하세요.
- 샘플에는 특정 게임명을 포함하지 않고 일반적인 값을 사용합니다.
