# Goldshell IEN616

This reference describes the IEN616 interface used by the tested Goldshell KA Box.
Its sources are captured SPI traffic, firmware inspection and physical mining tests.
It is community research, not a manufacturer specification.

## Test configuration

The tested miner is a Goldshell KA Box with one stock hashboard and firmware 2.2.2.
Its controller is `BoxII_CoreBoard V2.0`. Its hashboard is `LEP02_ComputeBoard V2`.
Stock software prepares the board before independent control.
These conditions define the tested setup. They do not define a requirement for every IEN616 design.

## Progress and remaining work

The tests show mining and automatic fan control on the stock KA Box board after
stock preparation. Independent cold boot and a new IEN616 board are untested.
The main remaining work is chip electrical mapping, supply startup, board
design and prototype tests.

| Outcome | Current status | Work required |
| --- | --- | --- |
| [SPI protocol](protocol.md) | Recorded packet forms and CRCs tested | Chip select and electrical limits need measurements. Other packet forms need tests. |
| [Stock-hashboard initialization](initialization.md) | 162-transfer recipe tested at 30 logical addresses after stock preparation | Measure the supply sequence and test independent cold boot. |
| [Register reference](registers.md) | Five selectors tested, with known fields and reset limits | Unknown fields and reset values need new observations. |
| [Mining and share validation](mining.md) | 121 accepted shares recorded in run 89 | Longer operation and other pool formats need tests. The host must reject invalid and stale results. |
| [Automatic cooling](stock-integration.md#two-fan-control) | Automatic control of both fans tested in run 89 | Individual fan control, sensor calibration and other thermal conditions need tests. |
| [Stop and recovery](operation.md) | Stop tested in run 89. Cold stock return tested in run 90. | Warm stock return failed in earlier tests. Other fault and recovery paths need tests. |
| [Chip electrical reference](unknowns.md#use-limits-and-reopening-conditions) | Package pins, rails and ratings unknown | Identify the package and pins. Measure the rails, IO levels and reference clock. |
| [Independent miner board](hardware.md#chip-interface-and-new-board-requirements) | No test recorded | Design the board and cooling. Test startup, valid PoW and recovery on a prototype. |

[Unknowns](unknowns.md#use-limits-and-reopening-conditions) gives the necessary
measurements and tests for each open item. The
[claim map](evidence.md#claim-map) gives the source IDs, check methods and
conditions for the results above. Recorded pool replies stay private.

## What works

The independent miner initializes the stock-prepared board, sends current
Kaspa work, checks returned results and submits accepted shares.
Run 89 recorded 121 accepted shares during 120.013 seconds of mining, with
automatic control of both fans. Run 90 showed a separate cold return to stock control.

The interface, startup data and register observations support a driver for the
tested stock board. Independent cold boot and a new miner board still need
chip identification, electrical measurements and supply preparation.

## Read this reference

1. [Protocol](protocol.md): command diagrams, byte order, replies and CRC.
2. [Initialization](initialization.md): ordered transfers, waits and response gates.
3. [Mining](mining.md): job, work, result, host PoW and pool acceptance.
4. [Registers](registers.md): bit maps, recorded values, tested effects and reset limits.
5. [Operation](operation.md): operating gates, Stop, faults and recovery.
6. [KA Box stock integration](stock-integration.md), [hardware](hardware.md) and
   [unknowns](unknowns.md): board controls, measured connections and new-board requirements.

[Task procedures](procedures.md) give the supported checks.
[Firmware observations](software.md) identify the matched software and its audited paths.
[Research notes](research-notes.md) explain allocation, retention and invalid results.
[Evidence](evidence.md) identifies sources and test limits.
The [figure index](figures/README.md) links editable diagrams and field metadata.

## Evidence labels

| Label | Meaning |
| --- | --- |
| `proven` | A physical observation supports the claim under its stated conditions. |
| `strongly indicated` | Observations support the claim, but another explanation is possible. |
| `hypothesis` | A possible explanation that needs a test. |
| `unknown` | The records give no answer. |

Firmware inspection shows software configuration. Offline replay checks saved
data. Neither check replaces a physical test or a recorded pool reply.

## Tested identity and topology

**C01/C13.** The matched KA Box `product.json` identifies `IEN616`.
The selected stock driver and command table agree with this configuration.
Goldshell IEN616 is the reference name, based on that firmware record.
Physical package marking, revision and chip manufacturer are unknown.

The findings apply to this tested miner. A matching firmware name alone does not qualify a different board.

| Item | Recorded value | Source and limit |
| --- | --- | --- |
| Miner | One Goldshell KA Box | C01. The reference assigns no public serial number. |
| Controller | `BoxII_CoreBoard V2.0` | C01. Recorded board marking. |
| Hashboard | `LEP02_ComputeBoard V2` | C01. Recorded board marking. |
| Installed firmware | Goldshell `2.2.2` | C16. Matched installed and extracted files. |
| Chip name | `IEN616` in `product.json` | C01. Package marking and revision are unknown. |
| Topology | 30 logical addresses, 32 reported cores at each address | C13. Physical package count is unknown. |
| Independent software | Native device owner and Rust PoW/pool path | C08. Research source stays in the private research repository. |
| Latest runtime | Run 89, source commit `d90b77476` | C08/C14/C15. Stock preparation is necessary. |
| Observation points | Console, native records, passive SPI, direct temperature and fan readings, pool replies, relay meter | Each point has a separate evidence limit. |

The documentation source baseline is
`c0bf48275d619bca1dbe4db5e8583153880b027e` in the private research repository.
Each run keeps its own source and capture identities.

## Release boundary

The documented interface supports a driver for the tested stock board after
stock preparation. A new board needs the electrical measurements and prototype
tests in [unknowns](unknowns.md#use-limits-and-reopening-conditions).
Unknown register fields have no assigned safe value.

The investigation closed on 2026-10-07 at the available measurement limit.
The [verification record](evidence.md#verification-record) gives the checks and their limits.

## Terms

| Term | Meaning |
| --- | --- |
| ASIC | Application-specific integrated circuit used for the mining calculation. |
| SPI | Serial Peripheral Interface between the controller and hashboard. |
| UART | Universal asynchronous receiver/transmitter used for the controller console. |
| GND | Ground connection. |
| TXD | UART transmit data from the controller. |
| RXD | UART receive data into the controller. |
| root shell | A controller command shell with user ID 0. |
| CRC | Cyclic redundancy check of specified packet bytes. |
| PoW | Proof of work. The host calculates it to check each candidate. |
| GPIO | General-purpose input/output at the controller. |
| ADC | Analog-to-digital converter. |
| TMP112 | Board temperature sensor in the tested controller path. |
| stock preparation | Board preparation by Goldshell software before independent control. |
| logical address | The one-byte source address on the tested interface. It does not prove a physical package. |
| core coordinate | The returned source-relative coordinate, from 0 to 31. It does not prove a physical core. |
| slot | One of the 15 nonzero work IDs carried by the protocol. |
