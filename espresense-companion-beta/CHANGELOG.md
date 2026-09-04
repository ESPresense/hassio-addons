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