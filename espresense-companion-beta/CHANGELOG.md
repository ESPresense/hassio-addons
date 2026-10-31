## 2.5.0

## What’s Changed

- feat(ui): Ctrl+K finder for everything (devices and nodes, tracked or not) @DTTerastar (#1773)
- build(deps-dev): Bump eslint from 10.12.0 to 10.13.0 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1779)
- build(ui): finish the SvelteKit 3 migration with sv migrate @DTTerastar (#1799)
- Bump ModelContextProtocol.AspNetCore from 2.2.0 to 2.3.0 @[dependabot[bot]](https://github.com/apps/dependabot) (#1784)
- ci: run Build and test on pushes to main @DTTerastar (#1798)
- build(deps): Bump paulhatch/semantic-version from 6.0.4 to 6.0.5 @[dependabot[bot]](https://github.com/apps/dependabot) (#1777)
- fix(ui): annotate node page props so svelte-check passes @DTTerastar (#1797)
- fix(devices): remove stale HA entities when a device's id alias changes (#1788) @DTTerastar (#1791)
- Bump Microsoft.AspNetCore.SpaProxy from 8.0.32 to 8.0.33 @[dependabot[bot]](https://github.com/apps/dependabot) (#1783)
- fix(locators): MultiFloor devices get a floor, so they show on the 2D map (#1605) @DTTerastar (#1787)
- build(deps-dev): Bump svelte-check from 4.7.6 to 4.7.7 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1781)
- build(deps-dev): Bump eslint-plugin-svelte from 3.23.0 to 3.23.1 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1782)
- Bump Polly from 8.8.0 to 8.8.1 @[dependabot[bot]](https://github.com/apps/dependabot) (#1785)
- build(deps-dev): Bump svelte from 5.57.1 to 5.57.3 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1780)
- build(ui): upgrade to SvelteKit 3 and Vite 8 (supersedes #1793) @DTTerastar (#1796)
- fix(locators): treat nearest\_node.max\_distance \<= 0 as no limit @DTTerastar (#1792)
- fix(config): make history.path and nearest\_node.max\_distance work; drop dead keys (#1767) @DTTerastar (#1786)
- fix(cleanup): expire devices restored from discovery without a retained last\_seen (#1789) @DTTerastar (#1790)
- refactor(mqtt): harden MqttCoordinator connect/reconnect and add mqtt.read\_only @DTTerastar (#1697)
- fix(dev): add ./watch so hot reload stops breaking the UI @DTTerastar (#1776)
- fix(ui): apply map.flip\_x / flip\_y to the 3D view (#1758) @DTTerastar (#1774)
- fix(mqtt): anchored devices report their anchor room, not not\_home @DTTerastar (#1761)

## 2.4.1

## What’s Changed

- fix(config): nook south wall on the LiDAR line @DTTerastar (#1770)
- feat(calibration): read v5 reports (rawRssi) and v5's rx\_offset\_rssi @DTTerastar (#1769)

## 2.4.0

## ⚠️ Breaking change: MQTT TLS certificates are now validated

With `ssl: true`, the broker's certificate is now checked by default (#1735). Setups that relied on a self-signed certificate used to connect and will now **fail to connect** until you configure one of:

| Setting | Behavior |
|---|---|
| nothing | Certificate must be trusted by the system CA store (e.g. Let's Encrypt). A failure logs how to fix it |
| `mqtt.ca_cert_path` | Certificate must chain to that CA |
| `mqtt.insecure: true` | Certificate is not checked (logs a warning) |

## Also in this release
- **Calibration:** continuous calibration now converges and `per_node_absorption` works (#1752, #1739, #1759).
- **MQTT:** read-only clients no longer register a last will (#1736).
- **UI:** Google Maps-style zoom on the map (#1762), coordinate readout fix (#1764).
- **Config:** corrected example floorplan (#1754).

## What’s Changed

- Add favicon for Paseo workspace icon @DTTerastar (#1768)
- fix(calibration): send 0 as 0; reset calibration with empty payloads @DTTerastar (#1765)
- fix(ui): map coordinate readout/copy offset by the padding since layercake 11 @DTTerastar (#1764)
- fix(config): correct the example floorplan from the robot's LiDAR map @DTTerastar (#1754)
- fix(ui): Google Maps-style map zoom (scroll/pinch, Shift+drag box zoom) @DTTerastar (#1762)
- build(deps-dev): Bump layercake from 10.0.3 to 11.0.0 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1746)
- build(deps): Bump the npm\_and\_yarn group across 1 directory with 3 updates @[dependabot[bot]](https://github.com/apps/dependabot) (#1760)
- build(deps-dev): Bump prettier from 3.9.6 to 3.9.10 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1744)
- build(deps-dev): Bump eslint-plugin-playwright from 2.12.0 to 2.12.1 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1743)
- Read-only MQTT client: skip the last will, document it in BUILDING.md @DTTerastar (#1736)
- build(deps-dev): Bump vite-plugin-devtools-json from 1.1.0 to 1.1.1 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1748)
- build(deps-dev): Bump svelte-check from 4.7.4 to 4.7.6 in /src/ui @[dependabot[bot]](https://github.com/apps/dependabot) (#1745)
- Bump Microsoft.AspNetCore.SpaProxy from 8.0.31 to 8.0.32 @[dependabot[bot]](https://github.com/apps/dependabot) (#1747)
- Bump Swashbuckle.AspNetCore from 10.2.3 to 10.3.1 @[dependabot[bot]](https://github.com/apps/dependabot) (#1757)
- fix(optimizers): per-node fit error and a warning when a value lands on a limit @DTTerastar (#1759)
- fix(optimizers): make continuous calibration converge, and make per\_node\_absorption work @DTTerastar (#1752)
- fix(sim): carry the device at a real height; add plug-height scenario @DTTerastar (#1751)
- test: bundle the anonymised robot survey so survey tests always run @DTTerastar (#1750)
- test: synthetic stationary recording and bruno node id @DTTerastar (#1741)
- fix: GaussNewton minimisation and the calibration acceptance gate @DTTerastar (#1739)
- Add capture replay scorer to the simulation harness @DTTerastar (#1595)
- feat(mqtt)!: validate the broker's TLS certificate by default @DTTerastar (#1735)

## 2.3.2


- fix(mqtt): retain companion `online` status so device trackers no longer stay `unavailable` after a Home Assistant restart (#1685) @YipYipYAML @DTTerastar (#1734)

**Full Changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.3.1...v2.3.2

## 2.3.1

## Fixes

- **UI stuck on the previous version after an add-on update** (#1733). The web UI shell was served without `Cache-Control`, so browsers (including the Home Assistant ingress frame) kept loading a cached `index.html` and the old bundle long after the container had updated. The shell is now cached for one minute and then revalidated, hashed assets are marked immutable, and the UI does a full page load on its next navigation once it sees a newer build.

### After this update

Browsers that cached the old shell under the previous policy will not contact the server until that cache expires. Do a hard reload of the Companion page once (right-click the frame → Reload frame, or Ctrl/Cmd+Shift+R while it has focus). From then on updates are picked up automatically.

## 2.3.0

## Highlights

- **Find any device.** Filter box on the Devices and Nodes tables; multiple words AND together, matches id, name, room and floor (#1731)
- **Anchor any device.** The calibration page now places the draggable spot at the floor centre for a device that has never been located, so untracked devices can be anchored (#1732)
- **Firmware updates that work.** Firmware listings come from espresense.com instead of the rate-limited GitHub API (#1725); the version picker no longer spins forever (#1722); node `update_error` is surfaced in node state and the UI (#1729); the MCP server can install firmware releases and PR builds, and stuck OTA jobs are fixed (#1726)
- **Node list shows each node's board** (#1727)
- **Configurable TLS certificate validation** for MQTT brokers with self-signed certs (#1686, thanks @theo416)

## Fixes

- MQTT password no longer leaks through the config API; all async event handlers are awaited (#1721)
- `exclude_devices` is now honoured on the autodiscovery path, so excluded devices stop resurrecting on restart (#1716)
- NaN guards in confidence, correlation and absorption math; settings-store round-trip gaps closed (#1699)
- Nodes with no floors assigned no longer break the map (#1714)
- Phase-0 bug fixes from the architecture assessment (#1692)

## Under the hood

- Frontend migrated to Svelte 5 runes (#1701)
- Typed `apiFetch` client, svelte-check at zero errors, lint/check/unit tests gated in CI (#1700, #1694, #1693)
- AutoMapper, dead optimizers and the frontend-test MSBuild hook removed (#1695)
- Docker image and devcontainer on Node 22 (#1723, #1728)
- Dependency bumps (dependabot)

## Known issues

- Firmware v5.0.0b0 nodes updating from GitHub release URLs can fail on the redirect; fixed in firmware v5.0.0b1

## 2.2.2

## Fixes

- **Calibration page no longer crashes the UI** (#1670, fixes #1664) — `crypto.randomUUID` is secure-context-gated and is missing in the insecure/older WebView contexts some Home Assistant installs run under. The tooltip action called it while attaching, so opening **Calibration** threw `crypto.randomUUID is not a function` and every other button in the companion stopped working until you reloaded. Tooltip ids now come from a plain counter. If you hit this on 2.2.1, this is your fix.
- **HA discovery now uses `availability_topic`** (#1663) — the MQTT LWT was being ignored, so entities didn't go unavailable when the companion dropped off.
- **`device_tracker` state topic is retained again, and cleared on delete** (#1634) — trackers survive a broker/HA restart instead of coming back blank.

## Internal

- Release dispatch waits for the Docker image to actually exist before firing (#1662), so the HA add-on update no longer races the image build.
- 12 dependency bumps (#1650, #1651, #1653, #1654, #1655, #1656, #1657, #1665, #1666, #1667, #1668, #1669).

**Full changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.2.1...v2.2.2

## 2.2.1

Maintenance release. **No runtime changes** — the companion behaves exactly as v2.2.0.

**Upgrading from 2.1.2?** Read the [v2.2.0 notes](https://github.com/ESPresense/ESPresense-companion/releases/tag/v2.2.0) first — that release changed how confidence and locator error are calculated, and made device `id:`/`name:` globs case-insensitive.

## Fixes

- **Docs**: `config.example.yaml` documented the `exponential` weighting `lambda` default as 3.0; the code default has been 1.5 since #1461. Comment now matches the code (#1661)
- **Release automation**: release events dispatched to `ESPresense/hass-addons`, which does not exist — the repo is `ESPresense/hassio-addons`. Every published release silently failed that step, so the Home Assistant add-on version was only ever bumped by the twice-weekly cron instead of at release time (#1660)

**Full changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.2.0...v2.2.1

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

## latest

This is just whatever is in espresense/espresense-companion:beta