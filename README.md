# webos-app-assets

Agent skill for generating and packaging LG webOS TV app visual assets (icons, splash screen, tile color) for TV game apps built with [cre-cli](https://github.com/creatodevs/cre-cli).

## Install

```bash
npx skills add Kyriakos-Michael/webos-app-assets -g -a cursor -y
```

Or install to the current project:

```bash
npx skills add Kyriakos-Michael/webos-app-assets -a cursor -y
```

## Use

In Cursor or any supported agent:

```
/webos-app-assets
Create icons for "My Game" — falling blocks, neon palette
```

The skill provides LG webOS asset specs, image-generation prompts, resize commands, and `appinfo.json` fields.

## What it covers

- 80×80 and 130×130 app icons (`icon.png`, `largeIcon.png`)
- 1920×1080 splash background (`splashBackground.png`)
- 400×400 Seller Lounge master icon (`store-assets/icon-400.png`)
- `iconColor` and `appinfo.json` branding fields

## License

MIT
