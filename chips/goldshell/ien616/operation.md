# IEN616 operation and recovery

## Ownership and tested operating state

**C14/C15.** The full runtime result is run 89 on a stock-prepared board.
The independent owner used current pool work while `kaspaminer`, `minerd`,
`xminerd` and `fanctrl` stayed stopped. It used direct board temperature and
fan speed data, with a separate relay watchdog.

The [startup diagram](figures/lifecycle.svg) shows the stock dependency and stop path.
This reference describes tested conditions. It does not authorize a new powered run.

| Runtime gate | Tested requirement | Failure action |
| --- | --- | --- |
| Device ownership | Identified stock processes stay stopped. Independent SPI owner stays identified. | Stop work and use the tested shutdown path. |
| Board temperature | `0 < T < 75` °C from direct TMP112 data | Stop at the bound or invalid reading. |
| Fan speed | Both tachometers at least 1,000 RPM | Stop if either fails. |
| Freshness | Direct health no older than 10 seconds | Stop when data is missing or stale. |
| Primary warning | Fresh direct health and valid fallback within 1 second | Stop if the fallback gate fails. |
| Secondary or SMBus failure | Operation must stop. | Stop. |
| Cooling lease | Fresh direct health, correct owners and correct fan readbacks | Independent watchdog cuts power after a failed lease. |

These are research test limits for the recorded board. They are not absolute
chip ratings or proof of safe unattended operation.

## Platform controls

The [KA Box stock integration](stock-integration.md) defines controller paths,
process ownership, power dependencies and the measured two-fan response.
Use those platform controls with the operating gates above.

## Stop, power cutoff and stock return

At run-89 completion, the independent owner released the device and read GPIO
66/67 LOW. Both fan-level readbacks were 100. A separate direct check confirmed
restoration. Volatile access closed.

The watchdog cut power after the parked lease. It verified OFF / 0 W / 0 A.
The parent independently recorded the same state.
No warm stock continuation was attempted.

Run 90 started separately from OFF. It applied no UART, SSH, GPIO, fan or SPI
command stimulus. Its physical capture contained all 30 admission replies,
25 stock work packets and 79 valid result packets.
The console recorded stock fan control. The run ended OFF / 0 W / 0 A.

This is a proven separate cold stock return. It is not an independent cold boot
of the new miner or proof of internal rail discharge.
The last state is historical evidence, not a current power check.

## Faults and failed gates

**C17.** Later passes do not change these earlier results.

| Run | Failed gate or observation | Effect on the reference |
| --- | --- | --- |
| 38/39 | Stock warm restoration failed after successful selected tests. | Keep ADC/filter results and the failed whole-run restoration. |
| 42 | 38 accepted shares, followed by failed warm stock return and a secondary temperature veto | No general warm-handoff claim. |
| 45 | Direct temperature reached 75 °C. Power cutoff followed. | Thermal stop passed. Planned endurance and fresh-owner stages did not complete. |
| 51/56/57/60 | Preparation, staging or adapter failures | No credit for the unexecuted physical contrasts. |
| 80 | Owner-command variable corruption prevented the first STOP. | No independent mining or cooling result. |
| 81 | Host closeout race caused runner exit 1. | Raw lifecycle and watchdog evidence shows the tested hardware result. The reporting fix was offline only. |
| 85 | A spinup sample contaminated the pre-step tachometer baseline. | Mining passed, but the frozen physical cooling gate failed. |
| 87 | A health frame lacked the fan-level field expected by the new baseline gate. | Failed lease caused power cutoff before an adaptive fan step. |

Separate cold stock returns followed the later cooling tests.
Run 89 passed with a stable pre-step baseline and a separate fan-level query.
The [run extracts](evidence/run-records.json) keep the source observations.
