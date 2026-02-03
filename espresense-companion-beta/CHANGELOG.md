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