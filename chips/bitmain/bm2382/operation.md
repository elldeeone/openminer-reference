# BM2382 operation and recovery

This page describes proven stock KS5 Pro operation. A new board needs separate
electrical and thermal qualification.

The test KS5 Pro has three hashboards. We used two for mining because the
test site could not power all three at once. The reported mining and power
results apply to that test configuration.

## Supported operating context

| Item | Tested context | Limit |
| --- | --- | --- |
| Topology | Stock controller; chains 0 and 2; 92 positions per chain | No arbitrary-chain or one-chip qualification |
| Startup | Complete stock-derived ramp and reset recipe | Final register values alone are insufficient |
| UART | Host 115,200 then 1,500,000; fast wire decoded at 1,562,500 | Not a chip IO-level specification |
| Frequency | Stock-derived 625 MHz class production recipe | Effective timing fit, not an absolute reference-clock measurement |
| Work feed | 81 ms two-chain pair policy | Not a throughput ceiling |
| Temperature | Stock ADC conversion; chain 2 first-owner hottest readings were 64–88 °C | Not calibrated silicon limits |
| Cooling | Four stock fans with active supervision | Not a new heatsink specification |
| Power | Recovery run whole-session maximum 2,493 W | Whole miner, not chip power or regulator current |
| Research reference | Eight 40 s fixed-job windows at 300 MHz | Separate workload and initialization scope |

During native mining, temperature telemetry came from all 92 positions on
chain 2. The reported hottest reading applies to that chain. It is not a
measured maximum across both mining boards. The controller polls that chain
once per second and requires a complete fresh inventory within 5 s.
Startup inventory checks cover both chains.

The KS5 supply command value `1500` is a controller value. It is not a
measurement of chip millivolts. Do not use it to select a carrier regulator.

## Required control sequence

1. Acquire exclusive ownership of UART, power, reset and fan control.
2. Establish bounded watchdogs, cooling and power limits.
3. Complete [stock integration](stock-integration.md), [cold initialization](initialization.md)
   and their response gates.
4. Feed fresh work. Check fresh temperature data and fan tachometers.
5. Software-validate every candidate before share submission.
6. On Stop or fatal fault, stop dispatch, disable ASIC power/reset through
   the qualified board control path, and confirm the terminal state.
7. Restore UART and stock service roles. Restore stock operation if required.
   Park with the qualified cooling policy, then power off.

## Physical validation

Two fresh-job runs exercised thousands of job changes. One accepted 79 shares
across 67 jobs. The other accepted 72 shares across 72 jobs. Neither reported
a rejected share. Passive UART evidence directly contains 40 and 39 of those
accepted results respectively. The capture does not contain every submission.

The recovery run sustained a first owner for 613.913 s. A planned pool EOF
caused a safe fault. A fresh owner then mined for 120.940 s and stopped safely.
Terminal GPIO readback showed power/reset controls 412, 427 and 431 at zero.
These GPIO numbers belong to the tested controller, not the ASIC package.
The readbacks confirm controller output states. They do not measure chip rail
voltage or its discharge time. Final relay OFF and wall 0 W / 0 A are
separate observations.

UART and role restoration completed. The first final stock park failed an
apparatus fan threshold check. A separate corrected closeout restored stock
operation, parked at 40% fan PWM and confirmed power off. This is a two-run
recovery result. Do not describe it as one uninterrupted successful run.

The other fresh-job run failed EOF closeout before a second owner could start.
Its successful mining evidence remains valid; its recovery is not proven.

See the [static validation summary](evidence/validation-summary.json).

## Remaining operating limits

Earlier higher-clock fixed-job tests contained 38 below-filter-score results.
Their cause remains unknown. The newer fresh-job run also recorded 69
below-`25` results in the original offline join. Three fail the returned-seed
check and cannot be bound to those dispatched jobs. The corrected join has
66 below-`25` results. These are different data sets.
The original join used prefix, work ID and age. The corrected join also
checks the seed. It still includes wire-retired jobs and is not a runtime
candidate inventory. Production rejects seed mismatches before PoW checking.
Successful software-valid shares do not resolve the fixed-job discrepancy.

Do not claim unattended operation, arbitrary warm recovery, voltage/frequency
optimization, a lossless hardware filter or a silicon thermal rating.

[BM2382 overview](README.md) · [Evidence](evidence.md) · [Unknowns](unknowns.md)
