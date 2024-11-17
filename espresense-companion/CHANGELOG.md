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
