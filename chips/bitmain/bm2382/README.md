# Bitmain BM2382

This reference documents the BM2382 interface used by the tested Antminer KS5 Pro.
It combines firmware observations, captured UART traffic and physical mining
tests. It is community research. It is not a manufacturer data sheet.

## Test configuration

The test miner was an Antminer KS5 Pro with three hashboards. We used two
hashboards for mining because the test site could not supply enough power
for all three. The two-hashboard setup describes these tests. It does not
define the miner's normal configuration or a BM2382 board-count requirement.

This reference describes the BM2382 chip interface. Operating settings and
measurements come from the tested KS5 Pro.

## Progress and remaining work

Stock-hashboard control is demonstrated on the tested KS5 Pro. A miner built
around an independent BM2382 board has not been demonstrated. The main
remaining work is chip electrical mapping, board design and prototype testing.

| Outcome | Current status | Work required |
| --- | --- | --- |
| [UART protocol](protocol.md) | Supported frames demonstrated and documented | Additional configurations need separate validation. |
| [Stock-hashboard initialization](initialization.md) | Demonstrated on two 92-position chains | Three-hashboard operation and other topologies remain untested. |
| [Register reference](registers.md) | 14 ASIC and four core targets partially characterized | Unknown fields remain documented. Complete internal characterization is optional research. |
| [Mining and share validation](mining.md) | Fresh-job mining and accepted shares demonstrated | Longer operation and other pool formats remain untested. |
| [Stop and recovery](operation.md) | Demonstrated within the stated test conditions | Broader fault and recovery coverage remains untested. |
| [Chip electrical reference](unknowns.md#required-for-a-new-miner-board) | Incomplete | Verify the package, pins, rails, IO levels and reference clock. |
| [Independent miner board](hardware.md#new-board-requirements) | Not demonstrated | Design the board and cooling. Demonstrate startup, valid PoW and recovery on a prototype. |

The [new-board requirements](unknowns.md#required-for-a-new-miner-board) state
the evidence needed to close each hardware gap. The
[remaining driver qualifications](unknowns.md#remaining-driver-qualifications)
define untested operating conditions. Keep
[optional internal research](unknowns.md#optional-internal-research) separate
from work required to build a miner.

## What works

`proven`: the tested stock controller can initialize two 92-position chains,
send changing Kaspa jobs, bind returned nonces, calculate PoW, submit accepted
shares and complete the documented stop/recovery path.

The protocol, exact startup data and partial register map support a stock-board
driver implementation. They do not yet specify a new one-chip miner board.
Package pins, rails, IO levels, reference clock and new-board thermal design
remain work in progress.

## Read this reference

1. [Protocol](protocol.md): frame layouts, byte order and checksums.
2. [Initialization](initialization.md): exact packets, waits, ordering and gates.
3. [Mining](mining.md): job → work → nonce → PoW → accepted share.
4. [ASIC registers](registers.md) and [core registers](core-registers.md):
   tested access, effects, reset scope and unknown fields.
5. [Operation](operation.md): supported conditions, stop and recovery evidence.
6. [Stock integration](stock-integration.md), [hardware](hardware.md) and
   [unknowns](unknowns.md): controller integration and
   the facts still required for a new board.

[Task procedures](procedures.md) give short enumeration, temperature and UART
transition instructions. [Editable diagrams](images/README.md) contain SVG sources and supplementary
field metadata.

[Research notes](research-notes.md) retain the predictive model, counter and
filter findings. [Evidence](evidence.md) describes the static extracts and
provenance. Read [contribution guidance](../../../CONTRIBUTING.md) before
adding a claim.

## Evidence labels

| Label | Meaning |
| --- | --- |
| `proven` | Physically tested observation, limited to its stated conditions |
| `strongly indicated` | Supported by multiple observations, with an alternative still open |
| `hypothesis` | Proposed explanation, not established |
| `unknown` | No supported answer yet |

A firmware value is labelled as firmware evidence. A recorded software replay
is labelled as software validation. These do not become physical proof by
being included in this reference.

## Tested identity and topology

| Item | Observation |
| --- | --- |
| Interface name | Configuration `BM2382`; chip type `0x2382` |
| Wire identity | Family bytes `23 82` |
| Stock miner | Antminer KS5 Pro |
| Installed hashboards | 3 |
| Mining hashboards in these tests | 2; site power limit |
| Tested chains | Controller chains 0 and 2 |
| Positions per chain | 92 |
| Address selectors | Even values `00` through `b6` |
| UART | 8 data bits, no parity, one stop bit |
| Low-speed setting | 115,200 baud |
| Fast host setting | 1,500,000 baud |
| Fast decoded wire rate | 1,562,500 baud |

These identify the tested interface. The physical package marking and silicon
revision are not confirmed. Do not transfer another Bitmain family's register
or electrical definitions to this chip.

## Release boundary

The public documentation can expose the known interface and invite contributors
without resolving every internal diagnostic. A complete independent hardware
reference still requires the electrical and mechanical facts in
[unknowns](unknowns.md#required-for-a-new-miner-board).

## Terms

| Term | Meaning |
| --- | --- |
| ASIC | Application-specific integrated circuit |
| UART | Universal asynchronous receiver/transmitter; the serial interface |
| CRC | Cyclic redundancy check; packet integrity check |
| PoW | Proof of work |
| ADC | Analog-to-digital converter; the returned sensor code |
| PWM | Pulse-width modulation; the fan control value |
| GPIO | General-purpose input/output; a controller signal |
| PLL | Phase-locked loop; the firmware frequency-control model |
| EOF | End of input; used here for pool disconnection |
