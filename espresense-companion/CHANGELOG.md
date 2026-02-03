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
