## 2.6.7

## What’s Changed

- fix(optimizer): add each link's own error to its weight in the per-node fit @DTTerastar (#1881)
- Bump ModelContextProtocol.AspNetCore from 2.3.0 to 2.3.1 @[dependabot[bot]](https://github.com/apps/dependabot) (#1875)
- build(deps-dev): bump the npm-minor-and-patch group in /src/ui with 12 updates @[dependabot[bot]](https://github.com/apps/dependabot) (#1876)

## 2.6.7-beta.3

beta.2 plus the spot judge stack, all unmerged (branch `beta/v2.6.7-beta.3`): #1877, #1878, #1879 and #1880, on top of #1881, #1865 and #1753 as in beta.2.

**Opt-in:** with no config change the optimizer decides exactly as in beta.2. It only adds a report.

- **Spot score (#1877, report-only).** Every cycle also scores the current calibration and each proposal on devices at known positions: anchors and taught locations. Each spot's level is fitted, so a device's rssi@1m guess doesn't count. See `baselineSpots` and `spots` in `get_optimizer_last_cycle`.
- **`optimization.judge: spots` (#1878).** Proposals are accepted on those spots instead of on node-to-node links. A proposal must be better on the spots consistently (sign test z ≥ 2) and gain at least 0.1 dB. It must not make node-to-node links more than 1 dB worse, and must not move the mean rxAdj more than 2 dB. With fewer than 4 spots the calibration is held.
- **Taught spots (#1879).** The same `fingerprints.json` the fingerprint locator uses: samples averaged per 0.25 m cell, counting only receivers that serve the spot's floor.
- **Spot Fit (#1880).** Under `judge: spots`, an optimizer that fits rxAdj and absorption on two thirds of the taught spots and is judged on the other third. Each cycle moves rxAdj by at most 3 dB and absorption by at most 0.2. The mean rxAdj is held where it was.

In a mirrored soak on one install, Spot Fit applied one step that held-out spots confirmed (spot RMS 4.79 → 4.56 dB), then held.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.7-beta.2

Beta of unmerged PRs #1881, #1865 (at 6a8f37d) and #1753 (at 4ec7831, as in beta.1) on main at c544389 = v2.6.6 (branch `beta/v2.6.7-beta.2`). beta.1 plus two optimizer fixes.

- **The per-node optimizer no longer lets steady links dominate (#1881, unmerged).** It weighted each link by 1 / rssiVar, and nodes report rssiVar from under 0.1 to over 500 dB², so a few steady links carried a third of the fit. A steady reading isn't an unbiased one, and replaying a house showed the resulting fits were worse than the calibration they would replace: the gate refused them every cycle. Each link's weight now includes its own error (16 dB²) as well as its reported variance. On the same replay the gate accepts the fit, and anchored devices located with multi_floor land about 1 m closer (2.5 → 1.5 m median). This affects `per_node_absorption`, the default mode.
- **device_residual's device term only applies in that mode (#1865, unmerged).** After switching from `device_residual` to another mode, older snapshots still carried device windows and kept scoring proposals with `device_weight` until they aged out.
- Otherwise as beta.1: the fingerprint locator (#1753) and the optional `device_residual` optimizer (#1865).

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.7-beta.1

Beta of unmerged PRs #1865 (at 1bde2c9) and #1753 (at 4ec7831) on main at c544389 = v2.6.6 (branch `beta/v2.6.7-beta.1`).

The code is the same as v2.6.6-beta.11. It's republished on the new version line now that v2.6.6 is stable, with both PRs at their current heads (#1753 was rebased; the only change since beta.11 is a test). The beta.11 tree also carried a stray `.pnpm-store`, and this one doesn't.

- **`device_residual` optimizer (#1865, unmerged).** Self-calibration from located devices' own residuals: a per-node rxAdj and trust and a per-device rssi@1m, with proposals scored on device data as well as node-to-node links (`device_weight`). It only runs with `optimization.optimizer: device_residual`. `optimization.device_residual.apply_device_ref` defaults to true, which overwrites manual device rssi@1m.
- **Fingerprint locator, robot survey and accuracy ruler (#1753, unmerged).** `locators.fingerprint` matches each node's raw rssi against a kernel-regressed survey surface instead of ranging; with `priority` (default on), a fingerprint fix above `min_confidence` is reported over ranging wherever the survey covers the device. Opt-ins: `fingerprint.tracker`, `fingerprint.prior`, `survey.robot`, `accuracy`, and room learning from HA corrections.
- **Readings from unplaced receivers are recorded (#1753).** They're fingerprint features; ranging locators leave them out.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6

## What’s Changed

- fix(optimizer): anchored-device measures use stationary receivers only @DTTerastar (#1870)
- fix(calibration): leave receivers that move out of anchored-device rows @DTTerastar (#1873)
- fix(optimizer): skip non-stationary nodes when reading measures @DTTerastar (#1871)
- fix(locators): solve on a snapshot of the readings; drop a solve a newer reading supersedes @DTTerastar (#1867)
- fix(optimizer): hold anchored devices' txRef at rssi@1m so anchors pin the gauge @DTTerastar (#1863)
- refactor(optimizers): remove the legacy chain and global\_absorption; per\_node\_absorption is the only mode @DTTerastar (#1862)
- feat(mcp): optimizer debugging tools (status, last cycle, residuals, measures, dry run) @DTTerastar (#1864)

## 2.6.6-beta.9

Beta of unmerged PRs #1865 (at 1bde2c9) and #1870 on main at 3f1168c (branch `beta/v2.6.6-beta.9`). beta.8 plus two fixes so that nodes that move stay out of calibration.

- **Device residuals use stationary nodes only (#1865, unmerged).** A node with `stationary: false` (a car, a node carried around) can have a point configured, but that point is only where it sometimes is. beta.8 still used its readings of devices, and placed it at that point, so whenever it was elsewhere every distance to it was wrong and the optimizer proposed an rxAdj for it. Node-to-node links already left such nodes out; device residuals now do too, including windows already collected.
- **Anchored-device measures use stationary receivers only (#1870, unmerged).** The same problem in the measures `per_node_absorption` fits: an anchored device was paired with every node that heard it and has a point, so the optimizer proposed absorption and rxAdj for a node that moves. This affects `per_node_absorption` as well as `device_residual`.
- Otherwise as beta.8. Held-out device windows are scored with each device's learned rssi@1m. `device_residual` only runs with `optimization.optimizer: device_residual`. `optimization.device_residual.apply_device_ref` defaults to true, which overwrites manual device rssi@1m.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.8

Beta of unmerged PR #1865 (at 7543b46) on main at 3f1168c (branch `beta/v2.6.6-beta.8`). beta.7 plus one fix to how `device_residual` scores proposals.

- **Held-out device windows are scored with each device's learned rssi@1m (#1865, unmerged).** In beta.7 they were ranged with the rssi@1m they were received with. With `apply_device_ref: false`, or for any device whose learned value never gets written, those can be several dB wrong. The device term then mostly measured that error, which no node proposal can fix. Now the baseline and every proposal are scored with the same learned values, so the term judges node calibration. In the optimizer's last-cycle report, `deviceWindows.evalRangedWithLearned` says how many held-out windows used a learned value.
- **Expect a lower held-out device residual and a solver scale near 1** in `get_optimizer_last_cycle`. The device-facing gains it reports are also smaller and more honest. A fix to one node shows only in the held-out snapshots where devices were near that node, so after the first step the every-snapshot gate can still refuse the rest, whatever `device_weight` is.
- Otherwise as beta.7: `device_residual` only runs with `optimization.optimizer: device_residual`. `optimization.device_residual.apply_device_ref` defaults to true, which overwrites manual device rssi@1m.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.7

Beta of unmerged PR #1865 on main at 3f1168c (branch `beta/v2.6.6-beta.7`). **#1830's room and floor hold is no longer in the BETA channel.** This beta is main plus #1865, and `filtering.max_velocity` is back to main's 0.5 default.

- **`device_residual` optimizer (#1865, unmerged).** Self-calibration from located devices' own residuals, so how a node hears devices in the room finally reaches the optimizer. It learns a per-node rxAdj and trust and a per-device rssi@1m, and scores proposals on device data as well as node-to-node links (`device_weight`). It only runs with `optimization.optimizer: device_residual`; in any other mode this beta behaves like main. `optimization.device_residual.apply_device_ref` defaults to true, which writes learned rssi@1m to devices and overwrites manual values. Set it to false to calibrate nodes only.
- **Locators solve on a fixed copy of the readings (#1867, merged)**, as in beta.5/beta.6.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.6

beta.5 with #1867 as merged into main at 3f1168c (branch `beta/v2.6.6-beta.6`):

- **#1867 as merged.** Since beta.5 it adds a lock so a solve's copy of a reading never mixes fields from two MQTT messages, and tests that drive the mid-solve abort through the real MathNet solvers. Behaviour is otherwise as in beta.5: each solve works on a fixed copy of the readings, and a solve that a newer reading supersedes is dropped quietly. A 33-minute A/B of beta.4 against beta.5 on mirrored MQTT had 47 `Maximum iterations` errors against 0, with the same positions and update counts. It's a correctness and log-noise fix, not an accuracy change.

Everything else is as in beta.5: unmerged #1830's room and floor hold, `max_velocity` default 0.5.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.5

beta.4 plus #1867 (branch `beta/v2.6.6-beta.5`):

- **Locators solve on one fixed set of readings, and drop a solve a newer reading supersedes (#1867).** MQTT was updating a device's readings while its locators were solving. Solves fitted a mix of old and new readings, and some never converged (`Error finding location ...: Maximum iterations (10000) reached`, about one a minute live). Each solve now takes a copy of the readings. If a newer reading lands mid-solve, the solve stops quietly; that reading has already queued the device for its next pass. After 3 dropped solves in a row, the next one runs to the end, so a device heard by many nodes still gets a position.

Everything else is as in beta.4: unmerged #1830's room and floor hold on main at 2614f3e, `max_velocity` default 0.5.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.4

beta.3 with one change (branch `beta/v2.6.6-beta.4`):

- **`filtering.max_velocity` defaults to 0.5 m/s again.** #1830 raised it to 1.5 (walking pace), which damps reported positions much less. This beta goes back to 0.5 while that is decided on #1830. A `max_velocity` set in `config.yaml` still wins.

Everything else is as in beta.3: unmerged #1830's room and floor hold on main at 2614f3e, including #1863 (anchors pin the calibration gauge).

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.3

Beta of unmerged PR #1830 on main at 2614f3e (branch `beta/v2.6.6-beta.3`). Everything in beta.2, plus:

- **Anchors pin the calibration gauge (#1863).** Anchored devices keep their txRef at their own rssi@1m. The optimizer no longer fits a txRef for them or publishes `tx_ref_rssi` to a device id. Proposals are no longer scored on anchor txRefs that nothing applies. With anchors present, the "every transmitter k dB hot = every receiver k dB deaf" ambiguity is settled by the anchors, so rxAdj can't drift along it.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.2

Beta of unmerged PR #1830 merged into main at c01aae2 (branch `beta/v2.6.6-beta.2`). Everything in beta.1, plus main since then:

- **Optimizer debugging over MCP and REST (#1864).** `get_optimizer_status`, `get_optimizer_last_cycle`, `get_optimizer_residuals`, `get_optimizer_measures` and `run_optimizer_dry_cycle` (and `/api/optimizer/*`). They show what the optimizer sees, what each proposal was, how it scored, and which gate refused it, with the numbers. The dry run applies nothing.
- **One optimizer mode (#1862).** `per_node_absorption` is the only mode. The legacy chain and `global_absorption` are removed.
- **#1830's branch since beta.1:** a replay harness over the robot-survey approach fixture (tests only).

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.6-beta.1

Beta of unmerged PR #1830, rebased on main at b0552d1.

- Smooth each fix once, keep up at walking pace, and start over after a gap.
- Hold the reported floor and room until a challenger earns the switch (`room_hold`, `room_margin` 0.25 m, `floor_hold`).

Robot-vacuum replay on the 2026-11-04 first-floor grid (30 dwells, against its merge-base): median error 2.49 m vs 2.62 m, right room 65% vs 64%, no-room fixes 11% vs 15%.

Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.5

## What’s Changed

- ci: beta and stable release channels with one Release workflow @DTTerastar (#1855)
- refactor: delete DeferredLoggerFactory; DeviceTracker and locators get injected loggers (#1846 follow-up) @DTTerastar (#1853)
- refactor: tracking rules and scenario factory out of State; one enabled-locator list (#1844) @DTTerastar (#1848)
- test(optimizers): score the position test on the solving locators @DTTerastar (#1755)
- refactor: separate scenario fusion from MQTT publishing and history in MultiScenarioLocator @DTTerastar (#1850)
- chore: one logging pattern and one JSON serializer per boundary (#1846) @DTTerastar (#1852)
- refactor(mqtt): split MqttCoordinator into connection, topic router, and message handlers @DTTerastar (#1851)

## 2.6.5-beta.1

## What’s Changed

- ci: beta and stable release channels with one Release workflow @DTTerastar (#1855)
- refactor: delete DeferredLoggerFactory; DeviceTracker and locators get injected loggers (#1846 follow-up) @DTTerastar (#1853)
- refactor: tracking rules and scenario factory out of State; one enabled-locator list (#1844) @DTTerastar (#1848)
- test(optimizers): score the position test on the solving locators @DTTerastar (#1755)
- refactor: separate scenario fusion from MQTT publishing and history in MultiScenarioLocator @DTTerastar (#1850)
- chore: one logging pattern and one JSON serializer per boundary (#1846) @DTTerastar (#1852)
- refactor(mqtt): split MqttCoordinator into connection, topic router, and message handlers @DTTerastar (#1851)


Pre-release: install the **ESPresense Companion - BETA** add-on, or run `espresense/espresense-companion:beta`.

## 2.6.4

## 🕸️ See your whole node mesh at a glance

The map has a new **Node links** toggle (bottom right). It draws a line between every pair of placed nodes that can hear each other, coloured by how well each link's measured distance matches the real one:

- 🔵 **blue**: measures short
- 🟢 **green**: about right
- 🔴 **red**: measures long

A node that's off on every link, or a wall that makes every link through it read long, now shows up immediately, without opening nodes one at a time. A few details:

- Anchored devices show up as dashed lines, so you can see how well each node hears a device at a known spot.
- Use **Nearest** (2/3/4/6/All) and **Show** (All/Long/Short) to cut the clutter.
- Hover a node to see all of its links, or hover a link to read `measured (real)` for both directions.
- Each link is judged by its *worse* direction, so a node that only transmits badly can't average itself green. (#1833)

## 🔗 Deep links work again

Refreshing, bookmarking or pasting a link like `/devices/<id>` or `/calibration/devices/<id>` no longer gives you a blank page, including under Home Assistant ingress. (#1831)

## ⚠️ Heads-up for MCP users

MCP tools and resources now return exactly the same JSON as the REST API. That means **camelCase** keys (`matrix`, `rmse`, `anchored` instead of `Matrix`, `RMSE`, `Anchored`), with nulls included and `NaN` written as `"NaN"`. If you have scripts or agents that read MCP output, update the key names.

On the plus side, MCP `get_calibration` now includes anchored-device rows plus best R/RMSE, just like the UI. (#1849, fixes #1836)

## 🛠️ Under the hood

- **Config loading is sturdier.** An invalid `config.yaml` no longer pins a CPU core or gets re-read every second. It's logged once and the last good config stays in use. Saving a setting from the UI no longer corrupts the next section of your config, and it keeps the file's permissions (it holds your MQTT password). (#1696)
- **One calibration engine.** REST and MCP now share a single `CalibrationService`, so they can't drift apart. (#1849)
- **Companion preview for contributors.** Point the UI dev server at any running companion with `COMPANION_URL` to try a UI branch on real data. (#1834)
- CI: a code owner's own PRs can now merge without a second reviewer. (#1835)

## 📦 Dependencies

Polly 8.9.0, three.js 0.187, plus a batch of grouped UI tooling bumps (Playwright, ESLint, Vitest, OpenLayers and friends). (#1820, #1821, #1823, #1824, #1825)

**Full Changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.6.3...v2.6.4

## 2.6.3

## Auto optimization

- **Proposals are validated on newer data**: auto optimization now fits on older snapshots and only applies a proposal if it also beats the current calibration on a newer holdout it never saw. Sampling, the training window and the optimization cadence now run on separate clocks (#1624).
- **Optimizer progress in the calibration view**: it now shows phase, snapshot and sample counts, last outcome, next run and lease holder, plus current and best-seen RMSE and correlation (#1624).

### Upgrade notes

- Companion now takes an optimization snapshot every 30 s, whatever `interval_secs` says. Snapshots stay in memory, so there are no extra database writes, but expect more memory and CPU during cycles.
- Each cycle fits and scores far more data: `training_window_mins` defaults to 30 minutes, or falls back to `keep_snapshot_mins` if that's set.
- `interval_secs` now only sets how often proposals are evaluated. Installs that don't set it go from 60 s to 15 minutes.
- Nothing is applied until the history covers half the training window.
- New optional keys: `sample_interval_secs`, `training_window_mins`, `optimization_interval_secs`, `validation_fraction` and `minimum_improvement`.

## Fixes

- **Device processing no longer repeats per node**: a device heard by several nodes used to be located once per message. It's now located at most once per pass, using all its current readings (#1698).
- **Clearing a retained discovery message now removes the device**: it previously removed nothing and logged `Device not found for deletion: (null)` (#1698).
- **Anchored devices keep their height on the calibration page**: `rssi@1m` was calculated at the default 1.0 m, and re-anchoring moved the device to that height (#1811).

**Full Changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.6.2...v2.6.3

## 2.6.2

## Fix

- **Devices start at 0,0,0 after a restart**: with `multi_floor` enabled, a device's first position was solved from the map origin, and it then slid slowly to its real location. Devices now appear in the right place straight away (#1812).

**Full Changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.6.1...v2.6.2

## 2.6.1

## Fix

- **Device page stuck on "Loading..."**: the Explain this fix panel from v2.6.0 failed for devices on a floor with no other room, such as a single-room floor. It now loads normally (#1810).

**Full Changelog**: https://github.com/ESPresense/ESPresense-companion/compare/v2.6.0...v2.6.1

## 2.6.0

## Explainable positioning

Every device page now shows how its position was worked out (#1794).

- **Explain this fix** replaces the Details panel. It shows which nodes heard the device, how each locator did, and the floor and room decision, with plain-language hints about what looks wrong.
- **On the map:** an uncertainty ellipse around the fix, a ring per node, and which nodes agreed or were rejected. Node pages draw their links, measured against the map.
- **API:** `GET /api/device/{id}/explain` returns the same trace.

The new panel doesn't show some solver diagnostics from Details, such as iterations, Pearson correlation and exit reason. `GET /api/device/{id}` still returns the old list, and #1809 tracks an expert mode for them.

## Fixes

- MultiFloor no longer reports a fix from a single node. (#1807)
- NearestNode picks the floor from the node's position. (#1805)
- Dropped the false "z outside floor" warning on nodes. (#1806)

## Maintenance

- The example config uses `multi_floor` and the robot-survey map fixes. (#1803)
- Pull requests are auto-assigned. (#1808)

**Full changelog:** https://github.com/ESPresense/ESPresense-companion/compare/v2.5.0...v2.6.0

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