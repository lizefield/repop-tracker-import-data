# Repop インポートJSON仕様

このドキュメントでは、Repop Tracker に取り込むためのインポートJSON仕様を説明します。

JSONには、ゲーム定義と、そのゲームに紐づくモンスター・ボス・イベントなどのデータを記載します。

## 基本構造

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

## `games` と `monsters` の関係

各モンスター・イベントは、いずれかのゲームに紐づきます。

`games[].name` がゲーム定義で、`monsters[].gameName` には対応するゲーム名を指定します。

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

インポート時の考え方:

- 同じ名前のゲームが既に存在する場合は、既存ゲームとして扱われます。
- 存在しない場合は、新規ゲームとして作成されます。
- モンスター・イベントは `gameName` によってゲームへ紐づきます。

## ルート項目

### `version`

JSON仕様のバージョンです。

```json
"version": 1
```

### `games`

インポート対象のゲーム一覧です。

```json
"games": [
  { "name": "Example Game" }
]
```

### `monsters`

インポート対象のモンスター・ボス・イベントなどの一覧です。

```json
"monsters": []
```

## ゲーム項目

### `name`

ゲーム名です。

```json
"name": "Example Game"
```

この値は `monsters[].gameName` から参照されます。

## モンスター / イベント項目

### `gameName`

このデータが属するゲーム名です。

```json
"gameName": "Example Game"
```

`games[].name` のいずれかと一致させます。

### `name`

モンスター・ボス・イベントなどの表示名です。

```json
"name": "Example Event"
```

### `tags`

フィルタや分類に利用するタグです。

```json
"tags": ["event"]
```

複数指定できます。

```json
"tags": ["boss", "field"]
```

### `respawnType`

リポップ種別です。

```json
"respawnType": "fixed"
```

指定できる値:

| 値        | 意味                                 |
| --------- | ------------------------------------ |
| fixed     | 曜日・時刻が決まっている定時リポップ |
| afterKill | 討伐記録から指定時間後にリポップ     |

### `notification`

通知設定です。

```json
"notification": {
  "enabled": true,
  "minutesBefore": [10]
}
```

項目:

| 項目          | 型       | 説明                       |
| ------------- | -------- | -------------------------- |
| enabled       | boolean  | 通知を有効にするか         |
| minutesBefore | number[] | リポップ何分前に通知するか |

### `isActive`

このデータを有効にするかどうかです。

```json
"isActive": true
```

無効データの扱いはアプリ側の実装に依存します。

## 定時リポップ設定

`respawnType` が `fixed` の場合は `fixedRule` を指定します。

```json
"fixedRule": {
  "weekdays": [1, 3, 5],
  "time": "21:00",
  "holdSecondsAfterRespawn": 10
}
```

### `weekdays`

リポップする曜日です。

| 数値 | 曜日 |
| ---: | ---- |
|    0 | 日   |
|    1 | 月   |
|    2 | 火   |
|    3 | 水   |
|    4 | 木   |
|    5 | 金   |
|    6 | 土   |

毎日の場合:

```json
"weekdays": [0, 1, 2, 3, 4, 5, 6]
```

### `time`

リポップ時刻です。`HH:mm` 形式で指定します。

```json
"time": "21:00"
```

### `holdSecondsAfterRespawn`

リポップ後に現在位置を維持する秒数です。

```json
"holdSecondsAfterRespawn": 10
```

## 討伐後リポップ設定

`respawnType` が `afterKill` の場合は `afterKillRule` を指定します。

```json
"afterKillRule": {
  "respawnMinutes": 1200,
  "holdSecondsAfterRespawn": 10
}
```

### `respawnMinutes`

討伐記録から何分後にリポップするかです。

```json
"respawnMinutes": 1200
```

### `holdSecondsAfterRespawn`

リポップ後に現在位置を維持する秒数です。

```json
"holdSecondsAfterRespawn": 10
```

## 補足

- ゲーム名・イベント名は言語ごとに異なる場合があるため、言語別JSONで管理します。
- 公開前にゲーム内スケジュールや公式告知を確認してください。
- サンプルには具体的なゲーム名を含めず、汎用的な値を使用します。
