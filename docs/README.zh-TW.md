# Repop 匯入 JSON 規格

本文說明用於匯入到 Repop Tracker 的匯入 JSON 規格。

JSON 中需要填寫遊戲定義，以及與該遊戲相關聯的怪物、首領、活動等資料。

## 基本結構

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

## `games` 與 `monsters` 的關係

每個怪物或活動都會關聯到某一個遊戲。

`games[].name` 是遊戲定義，`monsters[].gameName` 中指定對應的遊戲名稱。

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

匯入時的處理方式：

- 如果已存在相同名稱的遊戲，會作為既有遊戲處理。
- 如果不存在，會作為新遊戲建立。
- 怪物與活動會透過 `gameName` 關聯到遊戲。

## 根欄位

### `version`

JSON 規格的版本。

```json
"version": 1
```

### `games`

匯入對象中的遊戲列表。

```json
"games": [
  { "name": "Example Game" }
]
```

### `monsters`

匯入對象中的怪物、首領、活動等列表。

```json
"monsters": []
```

## 遊戲欄位

### `name`

遊戲名稱。

```json
"name": "Example Game"
```

此值會被 `monsters[].gameName` 參照。

## 怪物 / 活動欄位

### `gameName`

此資料所屬的遊戲名稱。

```json
"gameName": "Example Game"
```

需要與 `games[].name` 中的某個值一致。

### `name`

怪物、首領、活動等的顯示名稱。

```json
"name": "Example Event"
```

### `tags`

用於篩選和分類的標籤。

```json
"tags": ["event"]
```

可以指定多個標籤。

```json
"tags": ["boss", "field"]
```

### `respawnType`

重生類型。

```json
"respawnType": "fixed"
```

可指定的值：

| 值        | 含義                           |
| --------- | ------------------------------ |
| fixed     | 在指定星期與時間重生的固定重生 |
| afterKill | 從擊殺紀錄開始指定時間後重生   |

### `notification`

通知設定。

```json
"notification": {
  "enabled": true,
  "minutesBefore": [10]
}
```

欄位：

| 欄位          | 型別     | 說明                     |
| ------------- | -------- | ------------------------ |
| enabled       | boolean  | 是否啟用通知             |
| minutesBefore | number[] | 在重生前多少分鐘發送通知 |

### `isActive`

此資料是否啟用。

```json
"isActive": true
```

停用資料的處理方式取決於應用程式實作。

## 固定重生設定

當 `respawnType` 為 `fixed` 時，需要指定 `fixedRule`。

```json
"fixedRule": {
  "weekdays": [1, 3, 5],
  "time": "21:00",
  "holdSecondsAfterRespawn": 10
}
```

### `weekdays`

重生的星期。

| 數值 | 星期 |
| ---: | ---- |
|    0 | 日   |
|    1 | 一   |
|    2 | 二   |
|    3 | 三   |
|    4 | 四   |
|    5 | 五   |
|    6 | 六   |

每天的情況：

```json
"weekdays": [0, 1, 2, 3, 4, 5, 6]
```

### `time`

重生時間。使用 `HH:mm` 格式指定。

```json
"time": "21:00"
```

### `holdSecondsAfterRespawn`

重生後保持目前位置的秒數。

```json
"holdSecondsAfterRespawn": 10
```

## 擊殺後重生設定

當 `respawnType` 為 `afterKill` 時，需要指定 `afterKillRule`。

```json
"afterKillRule": {
  "respawnMinutes": 1200,
  "holdSecondsAfterRespawn": 10
}
```

### `respawnMinutes`

從擊殺紀錄開始，多少分鐘後重生。

```json
"respawnMinutes": 1200
```

### `holdSecondsAfterRespawn`

重生後保持目前位置的秒數。

```json
"holdSecondsAfterRespawn": 10
```

## 補充

- 遊戲名稱與活動名稱可能會因語言而不同，因此使用按語言區分的 JSON 進行管理。
- 發布前請確認遊戲內時程與官方公告。
- 範例中不包含具體遊戲名稱，而是使用通用值。
