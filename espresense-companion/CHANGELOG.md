## 2.2.1

Maintenance release. **No runtime changes** — the companion behaves exactly as v2.2.0.

**Upgrading from 2.1.2?** Read the [v2.2.0 notes](https://github.com/ESPresense/ESPresense-companion/releases/tag/v2.2.0) first — that release changed how confidence and locator error are calculated, and made device `id:`/`name:` globs case-insensitive.

## Fixes

- **Docs**: `config.example.yaml` documented the `exponential` weighting `lambda` default as 3.0; the code default has been 1.5 since #1461. Comment now matches the code (#1661)
- **Release automation**: release events dispatched to `ESPresense/hass-addons`, which does not exist — the repo is `ESPresense/hassio-addons`. Every published release silently failed that step, so the Home Assistant add-on version was only ever bumped by the twice-weekly cron instead of at release time (#1660)

**Full changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.2.0...v2.2.1

## 2.2.0

First release since v2.1.2 (Feb 2026) — 165 commits. Please read the top section before upgrading.

## ⚠️ Behavior changes to check before you upgrade

**Confidence values are calculated differently** (#1445)
All multilateralizers now share one confidence formula, built from normalized error, the Pearson correlation of measured vs. predicted distances, and how many of a floor's nodes are actually reporting. The numbers will not line up with 2.1.2. If you have Home Assistant automations or templates keyed on a confidence threshold, re-check those thresholds after upgrading.

**Locator error is now a normalized MSE** (#1445)
The objective changed from a weighted average of squared errors to a weight-normalized mean (`Σ wᵢeᵢ² / Σ wᵢ`). Reported `error` is on a different scale than 2.1.2, and solved positions can shift slightly — most visibly in setups where node weights are uneven.

**Device `id:` / `name:` globs are now case-insensitive** (#1659)
`devices:` and `exclude_devices:` patterns used to match case-sensitively while the rest of the pipeline matched case-insensitively. A pattern written in the wrong case did nothing before and takes effect now: an `exclude_devices:` entry may start excluding devices you were tracking, and a `devices:` entry may start tracking new ones. Worth a scan of your patterns before upgrading.

**New warnings for node `z` outside floor bounds** (#1635)
Node coordinates are absolute, but the docs used to say "within room", so a lot of configs have floor-relative `z`. Those nodes were **already** being silently dropped from locating on those floors — 2.2.0 just logs a warning at config load naming each one. No behavior change; you may simply find out that a node was never contributing. Fix by adding the floor's base elevation to `z`.

**`exponential` weighting default `lambda` is now 1.5** (#1461), down from 3.0. Only affects you if you set `algorithm: exponential`. Note `config.example.yaml` still documents 3.0 — the code is authoritative.

## New — all opt-in, off by default

- BFGS, MLE and multi-floor multilateralizers, `enabled: false` by default (#1452, #1445)
- Device capture recording with ground-truth markers for accuracy analysis (#1592)
- MCP node management and firmware update jobs (#1465) — this one writes to your nodes; try it on a single node first
- Calibration slider, and calibration table values rounded to 1 decimal (#1406, #1581)
- Multilateration simulation harness and an `ILocate` stdin/stdout JSON CLI (#1451, #1541, #1582)

## Fixes

- Host crash when MQTT disconnects during lease release (#1535)
- MQTT DNS failures no longer take the process down (#1483)
- `isAnchored` robustness in room floor display (#1391)
- Firmware artifacts now served from espresense.com (#1532)
- Measurement count added to the optimization warning (#1462)

## Under the hood

- UI moved to Skeleton v5 (#1642) — restyled, no functional change
- ~140 dependency updates

**Full changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.1.2...v2.2.0

## 2.1.2

## What's Changed
* Fix MQTT connection failures with older brokers by @sensiebot[bot] in https://github.com/ESPresense/ESPresense-companion/pull/1449

## Chores
* feat: add release dispatch workflow by @DTTerastar in https://github.com/ESPresense/ESPresense-companion/pull/1446
* feat: add PR labeler Action and include common branch prefixes by @DTTerastar in https://github.com/ESPresense/ESPresense-companion/pull/1448

## New Contributors
* @sensiebot[bot] made their first contribution in https://github.com/ESPresense/ESPresense-companion/pull/1449

**Full Changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.1.1...v2.1.2

## 2.1.1

- test: add comprehensive tests for tooltip action @DTTerastar (#1440)
- build(deps-dev): bump autoprefixer from 10.4.22 to 10.4.24 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1435)
- build(deps-dev): bump @floating-ui/dom from 1.7.4 to 1.7.5 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1434)
- build(deps-dev): bump svelte-check from 4.3.5 to 4.3.6 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1433)
- build(deps-dev): bump @typescript-eslint/parser from 8.53.0 to 8.54.0 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1436)
- Bump Swashbuckle.AspNetCore from 10.1.0 to 10.1.1 @[dependabot[bot]](https://github.com/apps/dependabot) (#1437)
- build(deps-dev): bump @testing-library/svelte from 5.2.9 to 5.3.1 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1438)

## 2.1.0

### Features & Improvements
- **Device Anchoring** - Add DeviceAnchor support and enhanced device anchoring functionality (#1288)
- **Model Context Protocol** - Implement MCP to expose application state, devices, and telemetry (#1353)
- **Configurable Home Assistant Discovery** - Discovery topic now configurable instead of hardcoded to "homeassistant" (fixes #1317) (#1346)
- Device Calibration UI improvements (#1260)
- Navigation reorganization (#1257)
- Skeleton v4 migration with accordion state alignment (#1326)
- NearestNode Locator - Initial implementation of fallback locator that  uses nearest node's room when trilateration isn't possible (e.g., only 1 or 2 nodes in range)

### Bug Fixes
- **Space Modal** - Fix space closing modal issue (#1262)

### Infrastructure
- Upgrade MQTTnet to v5 and refactor MQTT coordinator (#1328)
- Implement lease management system integrated into OptimizationRunner and MultiScenarioLocator (#1347)
- Add recovery firmware Playwright test coverage (#1390)
- **Chrome DevTools Integration** - Add Chrome DevTools integration for UI debugging (#1276)
- **Device Deletion** - Implement device deletion functionality and cleanup service (#1261)
- **MQTT Cleanup** - Clear retained MQTT messages on node deletion (#1264)
- **WebSocket Reconnection** - Implement reconnection logic with exponential backoff and jitter (#1258)

## 2.0.3

- UI upgraded to skeleton v3

## 2.0.2

- fix: sort devices by last seen timestamp @DTTerastar (#1229)
- feat(ui): use floating-ui tooltips on calibration page @DTTerastar (#1231)
- Refactor UpdateLocation method to remove confidence parameter @DTTerastar (#1230)
- ci: run frontend tests @DTTerastar (#1226)
- Upgrade LayerCake to v10 @DTTerastar (#1225)
- Make 3D view snazzy @DTTerastar (#1212)
- Remove svelte-table dep @DTTerastar (#1211)
- feat: render nodes with 3D logo model @DTTerastar (#1209)
- Improve firmware update UI @DTTerastar (#1189)
- Delete old nodes @DTTerastar (#1176)

## 2.0.1

- Work around Couldn't find a valid ICU package (#1168)

## 2.0.0

This is a big release, config has changed a bunch, check your config vs the example.
Contains a workaround for a regression in home assistant that stopped mqtt device trackers from showing the room
Contains Major Overhaul of Auto Optimization

- Fix not\_home update and add regression test @DTTerastar (#1159)
- Add GPS reporting toggle @DTTerastar (#1158)
- Fix Nelder-Mead locator naming typo @DTTerastar (#1155)
- Confidence improvements, now verifies correlation as well as error @DTTerastar (#1135)
- Major Overhaul of Auto Optimization @DTTerastar (#1121)
- Tweaks to RMSE and R @DTTerastar (#1113)
- RMSE and R @DTTerastar (#1111)
- Add rssi variance @DTTerastar (#1093)

## 1.0.24

- Redo the way navigation works/make device and node editing a model dialog @DTTerastar (#1077)
- Add rotation to config.example @DTTerastar (#1076)
- Add 3d detail when clicking device @DTTerastar (#1075)

## 1.0.23

This release fixes a bunch of issues w/ the 3d map. It also makes it possible to rotate your house so that latt/long are more accurate. Added a Geolocation view to help setup and align it.

- Use satellite imagery @DTTerastar (#1073)
- House Orientation for GPS (fixes #925) @DTTerastar (#1072)
- Fix 3d view origin (fixes #1004) @DTTerastar (#1071)
- 3d view fix cleanup @DTTerastar (#1070)
- Load and save name, fix null updating everything @DTTerastar (#1064)
- Misc rename, and fixes @DTTerastar (#1063)

## 1.0.22

- No https desired (fixes #1050) @DTTerastar (#1058)
- Fix fixes showing n/a when no scenario @DTTerastar (#1057)
- Svelte 5 Upgrade @DTTerastar (#1054)

## 1.0.21

- Improvements to optimization matrix @DTTerastar (#1048)
- Enhancements to firmware updating @DTTerastar (#1038)
- Improved error handling for configuration retrieval @DTTerastar (#1030)

## 1.0.20

- Fix initization race (fixes #637) @DTTerastar (#1029)

## 1.0.19

- Fix saving calibration (fixes #1027) @DTTerastar (#1028)

## 1.0.18

- Add Calibrate Device page, fix titles @DTTerastar (#1011)

## 1.0.17

- Add persistent LastSeen value to device @DTTerastar (#1002)

## 1.0.16

- Add Auto Update and Allow Prerelease toggles on Nodes page

## 1.0.15

- Add 3d view (#927)

## 1.0.14

- Locator configuration added @DTTerastar (#794)
- Fix misc error in MapCoordinates @DTTerastar (#869)

## 1.0.13

- Minor tweaks to copy coordinates functionality @DTTerastar (#867)

## 1.0.12

- Cmd-c/Cntrl-c to copy coordinates to clipboard @DTTerastar (#860)

## 1.0.11

- Fix hovering not working @DTTerastar (#858)
- Fix bug where +/- reset translation @DTTerastar (#857)

## 1.0.10

- Add keyboard handling to map w/ shift for precision @DTTerastar (#854)
- Smoother mouse wheel zoom when shift held @DTTerastar (#853/#855)

## 1.0.9

- Fix aspectRatio 1:1 @DTTerastar (#851)
- Faster build? @DTTerastar (#850)

## 1.0.8

- Fix coordinates @DTTerastar (#847)

## 1.0.7

- Add coordinates to map @DTTerastar (#845)

## 1.0.6

- Increase zoom range @DTTerastar (#843)

## 1.0.5

- Background upload @DTTerastar (#841)
- Add configurable wall thickness, color, opacity @DTTerastar (#829)
- Add flipX and flipY config options (fixes #253) @DTTerastar (#830)

## 1.0.4

- Fix 404 on calibration fixes #585 @DTTerastar (#803)
- Add NadarayaWatsonMultilateralizer @DTTerastar (#754) (not yet usable tho)
- Nicer Dockerfile @DTTerastar (#773)

## 1.0.3

- Fix calibration order, add reset @DTTerastar (#767)
- Npm upgrades @DTTerastar (#765)

## 1.0.2

- Allow for excluding id patterns @DTTerastar (#751)
- ShowUntracked checkbox @DTTerastar (#755)
- UI Improvements @DTTerastar (#752)

## 1.0.1

- Load devices from mqtt at startup (fixes #336) @DTTerastar (#749)
- Add .devcontainer file with DotNet 8.0 and Node 20 @leccelecce (#730)
- Add IP address column to Nodes table @leccelecce (#729)
- If mqtt client id contains read don't write to mqtt @DTTerastar (#728)

## 1.0.0

- Bayesian approach for scenario selection by @DTTerastar in #697
- Add Kalman Filter by @DTTerastar in #698
- Fix HASS discovery, set source_type to bluetooth fixes #657 by @DTTerastar in #699

## 0.6.5

- Fix GaussianWeighting + Refactors @DTTerastar (#568)

## 0.6.4

- Npm upgrades @DTTerastar (#532)
- Bump Serilog.AspNetCore from 8.0.0 to 8.0.1 in /src @dependabot (#530)
- Bump Flurl.Http from 3.2.4 to 4.0.2 in /src @dependabot (#529)

## 0.6.3

- Fix github artifacts updating @DTTerastar (#513)

## 0.6.2

- Rollback Flurl @DTTerastar (#495)
- Bump @typescript-eslint/parser from 6.15.0 to 6.16.0 in /src/ui @dependabot (#492)
- Fix supervision mqtt auth @DTTerastar (#494)

## 0.6.0

- Websocket updating of devices, radar for nodes, node detail @DTTerastar (#459)

## 0.5.2

- Add variance to radar @DTTerastar (#444)
- Fix saving empty rssi@1m @DTTerastar (#445)

## 0.5.1

- Add variance reporting to calibration screen @DTTerastar (#443)

## 0.5.0

- Adding tests, move to single connection @DTTerastar (#435)

## 0.4.9

- Upgrade to net8 @DTTerastar (#423)

## 0.4.8

- Fix aspect ratio @DTTerastar (#407)

## 0.4.7

- Fix issues where devices can get stuck @gunnarbeutner (#392)

## 0.4.6

- Add master builds to artifacts dropdown @DTTerastar (#391)

## 0.4.5

- Much faster updating @DTTerastar (#390)
- Fix showAll @DTTerastar (#388)

## 0.4.4

- Fix issue doing second firmware update @DTTerastar (#382)

## 0.4.3

- Fixes: config causes a crash #338 @DTTerastar (#370)

## 0.4.2

- Less dumb logs @DTTerastar (#364)
- Close on firmware success @DTTerastar (#363)

## 0.4.1

- Use websockets instead of streaming for HA Addon @DTTerastar (#362)

## 0.4.0

- Built-in firmware updater (in nodes) @DTTerastar (#332)

## 0.3.40

- Don't show nodes as devices if configured with location and stationary @DTTerastar (#309)
