# IEN616 unknowns

## Scope of the chip findings

The test records apply to IEN616 operation on the specified KA Box boards.
They do not qualify every miner with the same firmware name.
Physical package identity and independent-board operation need the measurements below.

## Use limits and reopening conditions

The accessible investigation is closed. Each item below stays open at the
stated boundary. A new test must have a new observation or control that can
distinguish the competing explanations.

| ID | Unknown | Effect on use | Evidence that can reopen the item |
| --- | --- | --- | --- |
| U01 | Package marking, revision, count, orientation and dimensions | No verified package identity or footprint | Scaled cold photographs with each package visible and readable markings. |
| U02 | Pinout, connector continuity and internal ground domains | No independent wiring or carrier design | Cold continuity and diode/resistance measurements with shown local references. |
| U03 | Rails, supply sequence, current limits and discharge | No independent cold boot or safe new-board supply design | A target-bound sequence with measured voltage, current and timing, plus reversible shutdown. |
| U04 | Absolute clock, IO limits, waveform quality and electrical ratings | No absolute MHz or unrestricted tuning claim | Suitable isolated measurements at identified test points and attributed ratings. |
| U05 | Directed reversible individual-core control | Strict BM2382 behavioral parity stays incomplete | A target command or field with same-work sibling controls and restoration with unchanged values. |
| U06 | Persistent ASIC emitted-result counter, clear and resume | Host counts cannot measure all internal or overwritten results | An ASIC-visible value whose interval delta matches physical emitted frames, then clear/resume proof. |
| U07 | Reset-only register values and undiscovered fields | No default values for unresponsive state or arbitrary writes | A tested observation path before initializer writes, or a new source-backed register interface. |
| U08 | General warm stock return | Stop and cold return stay separate tested paths | A new reversible transition with explicit ownership, health and restoration gates. |
| U09 | Internal retention circuit and variable invalid-result cause | No internal FIFO or deterministic invalid-emission claim | An independent state/source control that distinguishes proposed mechanisms. |
| U10 | Nonzero result trailing byte and arbitrary late slot reuse | Reject unsupported forms and keep job ownership | Captured, assigned results with an independent timestamp or stale-result discriminator. |
| U11 | Full-cycle recurrence and arbitrary work constructors | Selected wrap does not show unlimited cyclic search | A shorter source-backed cycle field, checkpoint or independently observable state. |
| U12 | Independent fan actuation, sensor calibration and junction temperature | Cooling of the two fans only | Electrical fan mapping and calibrated thermal measurements. |
| U13 | Persistent SSH access, installed replacement image and full recovery | Research runtime is not installed replacement firmware | Tested boot, update, rollback and recovery with independent supply readiness. |
| U14 | Minimum physical population and independent board operation | No isolated-chip or new-board qualification | Measured necessary nets and a separate board test with no unsupported electrical assumptions. |
| U15 | Other faults, ambient conditions and long-duration operation | Short tests do not show unattended endurance | A defined operating envelope and a new bounded test for a concrete failure prediction. |
| U16 | Order of TXD and RXD at the two round J2 pads | The photo does not give a verified wiring order | A recorded connection or signal measurement that identifies each pad. |

## Investigation boundary

The selected installed paths, recorded captures, official firmware and source
searches found no accepted control for U05 or U06.
Absence from those sources does not prove absence in silicon.
The readable 32-core census has recorded evidence. It is not individual-core control.

New instruments and safe measurement points can reopen U01-U04 and U12-U14.
The existing GPIO names, meter values and packet counts cannot substitute for
those measurements.

The [closure record](evidence/source-catalog.json) is source `CLOSURE`.
The [question extract](evidence/research-coverage.json) keeps each Q01-Q38 row,
including Q03R. The [firmware extract](evidence/firmware-coverage.json) keeps
the examined source boundaries. The current overview uses the last closure,
not historical pending-task text.

## Evidence access and publication

Source captures, private pool replies and full software sources stay in
controlled research storage. Their hashes identify the data but do not give
access. The reference supplies selected packet, result, health and source extracts.

An independent reviewer can repeat the published offline calculations.
Physical reproduction must have the full research records, the tested unit,
appropriate instruments and a separately approved procedure.
Public release also must have the repository's publication review.
