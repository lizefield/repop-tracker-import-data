# Repop 导入 JSON 规范

本文档说明用于导入到 Repop Tracker 的导入 JSON 规范。

JSON 中需要填写游戏定义，以及与该游戏关联的怪物、首领、活动等数据。

## 基本结构

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

## `games` 与 `monsters` 的关系

每个怪物或活动都关联到某一个游戏。

`games[].name` 是游戏定义，`monsters[].gameName` 中指定对应的游戏名称。

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

导入时的处理方式：

- 如果已存在相同名称的游戏，则作为已有游戏处理。
- 如果不存在，则作为新游戏创建。
- 怪物和活动通过 `gameName` 关联到游戏。

## 根字段

### `version`

JSON 规范的版本。

```json
"version": 1
```

### `games`

导入对象中的游戏列表。

```json
"games": [
  { "name": "Example Game" }
]
```

### `monsters`

导入对象中的怪物、首领、活动等列表。

```json
"monsters": []
```

## 游戏字段

### `name`

游戏名称。

```json
"name": "Example Game"
```

该值会被 `monsters[].gameName` 引用。

## 怪物 / 活动字段

### `gameName`

该数据所属的游戏名称。

```json
"gameName": "Example Game"
```

需要与 `games[].name` 中的某个值一致。

### `name`

怪物、首领、活动等的显示名称。

```json
"name": "Example Event"
```

### `tags`

用于筛选和分类的标签。

```json
"tags": ["event"]
```

可以指定多个标签。

```json
"tags": ["boss", "field"]
```

### `respawnType`

刷新类型。

```json
"respawnType": "fixed"
```

可指定的值：

| 值        | 含义                           |
| --------- | ------------------------------ |
| fixed     | 在指定星期和时间刷新的固定刷新 |
| afterKill | 从击杀记录开始指定时间后刷新   |

### `notification`

通知设置。

```json
"notification": {
  "enabled": true,
  "minutesBefore": [10]
}
```

字段：

| 字段          | 类型     | 说明                     |
| ------------- | -------- | ------------------------ |
| enabled       | boolean  | 是否启用通知             |
| minutesBefore | number[] | 在刷新前多少分钟发送通知 |

### `isActive`

该数据是否启用。

```json
"isActive": true
```

禁用数据的处理方式取决于应用实现。

## 固定刷新设置

当 `respawnType` 为 `fixed` 时，需要指定 `fixedRule`。

```json
"fixedRule": {
  "weekdays": [1, 3, 5],
  "time": "21:00",
  "holdSecondsAfterRespawn": 10
}
```

### `weekdays`

刷新的星期。

| 数值 | 星期 |
| ---: | ---- |
|    0 | 日   |
|    1 | 一   |
|    2 | 二   |
|    3 | 三   |
|    4 | 四   |
|    5 | 五   |
|    6 | 六   |

每天的情况：

```json
"weekdays": [0, 1, 2, 3, 4, 5, 6]
```

### `time`

刷新时间。使用 `HH:mm` 格式指定。

```json
"time": "21:00"
```

### `holdSecondsAfterRespawn`

刷新后保持当前位置的秒数。

```json
"holdSecondsAfterRespawn": 10
```

## 击杀后刷新设置

当 `respawnType` 为 `afterKill` 时，需要指定 `afterKillRule`。

```json
"afterKillRule": {
  "respawnMinutes": 1200,
  "holdSecondsAfterRespawn": 10
}
```

### `respawnMinutes`

从击杀记录开始，多少分钟后刷新。

```json
"respawnMinutes": 1200
```

### `holdSecondsAfterRespawn`

刷新后保持当前位置的秒数。

```json
"holdSecondsAfterRespawn": 10
```

## 补充

- 游戏名称和活动名称可能因语言而不同，因此使用按语言区分的 JSON 进行管理。
- 发布前请确认游戏内时间表和官方公告。
- 示例中不包含具体游戏名称，而是使用通用值。
