# IEN616 firmware observations

## Source identity

**C16.** The installed firmware is Goldshell KA Box 2.2.2.
The matched `kaspaminer` file has 602,128 bytes and SHA-256:

```text
c2453674c294074e56db76ad62c3ab3dc46ad9d60d3f25f31c44891fb3a9d2f1
```

The matched `product.json` has SHA-256:

```text
3cc05a3111ed2b23cc1fedcd48e4557dd83c3fd7c0729111135f022623e99213
```

The [source catalog](evidence/source-catalog.json) gives the remaining file identities.
The reference includes protocol data and technical findings. It includes no
firmware binary, decompiler output, executable driver or bench-control tool.

The selected driver descriptor names `intchains_qomo`. Its KA command table
contains 15 rows. Other compiled families have different tables.
A compiled helper without a selected call path is not a tested chip capability.

## Stock process responsibilities

[KA Box stock integration](stock-integration.md#stock-process-responsibilities)
records the four stock process roles and the tested ownership conditions.
The source audit below gives their examined behavior and limits.

## Firmware audit coverage

The [firmware coverage extract](evidence/firmware-coverage.json) keeps all
30 audit rows, their observations and limits. The table below groups their
results by subject. These are source inspections or offline execution unless
a separate physical run is named.

| Audit rows | Finding | Limit |
| --- | --- | --- |
| F01 | Framing, CRC, padding, receive alternatives, SPI callback, retries and result drain | Physical chip select and unused command forms stay unknown. |
| F02 | Selected command table, five direct register selectors and register constructors | Source field operations do not show each silicon meaning. |
| F03 | Startup, retry, fallback and cleanup paths | Stock preparation is necessary. Degraded stock success is not independent admission. |
| F04 | Work construction, hash, job admission, slot lifetime, sessions, timers and accounting | Selected source paths and firmware-instruction cases. Changed topology and arbitrary inputs stay open. |
| F05 | ADC, chip sensors, TMP112, fan writers and thermal shutdown | Conversion agreement is not sensor calibration. |
| F06 | Selected time sources, waits and runtime deadlines | Software timing is not a measured ASIC clock. |
| F07 | Management and console ownership | No installed replacement image or universal recovery route. |
| F08 | Volatile SSH and root access, with recorded closure | Access machinery does not define chip behavior or persistent recovery. |
| F09 | Register access, relative clock path, completion flag and counter search | No independent ASIC result counter or core actuator found. |
| F10 | Installed dependencies, child commands, mapped configuration and watchdog startup | Generic library/kernel behavior and atomic mounted state stay outside the selected checks. |
| F11-F13 | KA Box 2.2.9, 2.2.7, boot/update data and related IEN616E firmware | Comparisons were offline. Related-chip fields are not target-tested controls. |
| F14 | 12,832 full mapped JSON pages in 14 schemas | The recorded flash mosaic is not one atomic filesystem image. |
| F15 | Six official KAS archives and eight controller roots | Dated public-source coverage only. |
| F16 | Selected cloud update delivery and signed update path | Authenticated backend selection and live delivery were not recorded. |
| F17 | Eleven diagnostic HTTP routes and host error/performance accounting | Host counters are not ASIC emission counters. |
| F18 | Forty-six local mining API commands | Selected device callbacks expose no ASIC counter/clear or per-core actuator. |
| F19 | Recorded 32 MiB flash image and residual artifact search | No new executable interface found. Encoded or absent content stays outside the result. |
| F20 | Twenty-seven transport entries and 206 decoded indirect calls | Compiler-generated reachability only. Arbitrary address synthesis is not included. |
| F21 | 218 CRC-valid chip-sensor responses in 74 captures | The source consumes temperature data, not an emission counter. |
| F22 | Stock BIST and 100 full following core-count sequences | Broadcast or whole-source operation. No per-core selector. |
| F23 | 509,161 transfers and 507,001 accepted frames in 74 captures | No persistent counter field found in the accepted frame set. |
| F24 | Five CRC failures, 802 unknown markers and 1,127 residual spans | Request-word artifacts and malformed data stay distinct from valid results. |
| F25 | 205 current firmware archives across the official family tree | No new target-compatible command table found. |
| F26 | 202 reachable commits and 217 unique archive blobs | Unreachable, private and future firmware are outside the result. |
| F27 | Dated official web, support and download content | No new target firmware found in the selected anonymous content. |
| F28 | Specified GoldshellZone 3.1.0 client | Its update action is cloud control, not an IEN616 opcode. |
| F29 | Dated client distribution records | Store version data does not show new ASIC commands. |
| F30 | Full recorded work/result coordinate audit | Predictive attribution does not give per-core control or a counter. |

The later closure record determines the last investigation status.
Historical audit headings can still describe work that was pending at their date.

## Work construction and accounting findings

The F04 constructor checks covered 49 cases, seven saved stock payloads and
31 work-ID/control cycles. The selected host IDs cycle from 1 through 15.
The host keeps recorded work ownership independently of the returned slot.

The lifetime audit distinguishes benchmark accounting from network accounting.
The timer/HTTP audit shows that retry states can bypass terminal accounting.
Terminal states feed a deferred outstanding-count reduction before last cleanup.
The session and failover audits keep their specified ownership and configuration limits.
These findings describe the matched software, not a new network test.

The all-zero pre-PoW seed has a separate software failure. The selected stock
matrix generator again and again produces rank-zero matrices and has no attempt limit.
The offline run stopped at its external instruction limit.
The independent Rust implementation instead returns `MatrixGenerationLimit`.

The job-admission audit found a conditional source path from malformed or zero
input to work construction. A returned candidate would enter the hash loop.
Recorded zero-seed ASIC output and installed network behavior stay unknown.
No live exploit, hardware stall or recovery result follows from this offline finding.

## Sensor and thermal software

The matched TMP112 conversion swaps the SMBus word, extracts a signed 12-bit
value and multiplies it by 0.0625 °C. A read error returns `-150` °C.
The product selects bus 0, address `0x48`. These are controller access values.

The chip-temperature source uses the following recorded conversion:

```text
-8.929e-12*x^4 + 6.5714e-8*x^3 - 0.00018002*x^2 + 0.33061*x - 60.927
```

Raw zero gives approximately -60.927 °C in that code. It is an invalid converted
sample, not evidence of cold silicon. Channel placement and calibration stay unknown.

The product configuration contains target 75 °C and cutoff values 89/105 °C.
The selected `fanctrl` path uses 89 °C. It samples at six-second intervals and
acts after six consecutive high observations. Its supply-disable helper makes
at most five calls. These source values are not the independent test limits.

Primary temperature failure can use valid secondary data. If the two are invalid,
the selected `minerd` path requests full fan speed. Some `fanctrl` invalid-sensor
behavior depends on supervisor PID-file presence.
The independent [operation contract](operation.md) keeps direct health and its
stricter stop gates.

## New software and firmware limits

Firmware 2.2.9 removes the selected register-3 completion wait in its scan path.
It also contains a changed register-6 sequence. These differences do not expose
the missing per-core control or ASIC counter.
This reference uses the saved firmware. No firmware update occurred.

The [stock UART console](hardware.md#j2-uart-console) gives root access without
a password prompt. The research SSH route uses temporary files in RAM.
Persistent SSH access, a recovery image, independent supply initialization
and an installed replacement remain untested.
The [unknowns page](unknowns.md) separates these deployment needs from runtime proof.
