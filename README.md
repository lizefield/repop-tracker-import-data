# Repop Tracker Import Data

This repository provides import JSON files for Repop Tracker.

## Documentation

- [English](./docs/README.en.md)
- [日本語](./docs/README.ja.md)
- [한국어](./docs/README.ko.md)
- [简体中文](./docs/README.zh-CN.md)
- [繁體中文](./docs/README.zh-TW.md)

## Directory Structure

```txt
games/<game>/<language-code>/import.json
```

Example:

```txt
games/example-game/en/import.json
games/example-game/ja/import.json
games/example-game/ko/import.json
games/example-game/zh-CN/import.json
games/example-game/zh-TW/import.json
```

## Language Codes

Recommended language codes:

| Code  | Language            |
| ----- | ------------------- |
| en    | English             |
| ja    | Japanese            |
| ko    | Korean              |
| zh-CN | Simplified Chinese  |
| zh-TW | Traditional Chinese |

Use `zh-CN` and `zh-TW` when Simplified Chinese and Traditional Chinese should be managed separately.

## File Naming

Use `import.json` as the standard file name.

```txt
games/<game>/<language-code>/import.json
```

Keeping the file name stable makes it easier to reference the file from apps, documentation, and raw GitHub URLs.

## License

See [LICENSE](./LICENSE).
