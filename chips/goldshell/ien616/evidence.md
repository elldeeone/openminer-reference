# IEN616 evidence

## Source catalog

The [source catalog](evidence/source-catalog.json) gives each source ID, relative
research path, SHA-256, byte count, evidence type and access limit.
The source baseline is `c0bf48275d619bca1dbe4db5e8583153880b027e`.
The tested unit is the KA Box with `BoxII_CoreBoard V2.0`,
`LEP02_ComputeBoard V2` and installed firmware 2.2.2.

The baseline identifies the documentation inputs. It does not replace each
run's source/build identity. Related firmware comparisons are offline evidence
for the specific images named in their source rows.

| Source IDs | Type and observation point | Included extract | Access limit |
| --- | --- | --- | --- |
| `CLOSURE`, `STATE` | Last research decision and latest unit state | Overview and capability tables | Full working records are private. |
| `COVERAGE` | Q01-Q38 and Q03R research questions | [All question rows](evidence/research-coverage.json) | Historical wording can describe an earlier state. |
| `FIRMWARE-COVERAGE` | F01-F30 source audits | [All firmware rows](evidence/firmware-coverage.json) | Source inspection is not a new hardware test. |
| `COLD`, `CONTACTS`, `CONTACT-PHOTO`, `CONTACT-RECEIPT` | Photographs and operator observations | [Contact photo](evidence/connector-contacts.png), [annotation record](evidence/connector-annotation.json) | Measurement records and other photographs stay private. |
| `PHOTO-CONTROLLER`, `PHOTO-ASSEMBLY`, `PHOTO-REVERSE`, `PHOTO-AUDIT` | Source photographs and their inspection record | [Board views](hardware.md#board-views) | Three source photographs and two views with margins removed. Heatsinks hide the ASIC packages. Owner review is necessary before public release. |
| `R001-UART`, `R001-ACCESS` | Recorded console response and physical test summary from attempt 01 | [Selected console records](evidence/j2-console.json) | Root access through the tested stock UART. Other access paths need their own evidence. |
| `OPERATOR-J2`, `PHOTO-J2` | Operator pad identification and supplied photograph detail, 2026-10-08 | [J2 detail](evidence/j2-detail.png), [operator report](evidence/j2-console.json) | The operator identified the square pad as ground. TXD/RXD order is unconfirmed. |
| `RECIPE-DATA`, `STARTUP-OWNER`, `R089-NATIVE` | Selected initializer and recorded native transfers | [Complete recipe](evidence/initialization.tsv), [command exchanges](evidence/command-examples.json) | Rounded waits apply after each transfer. Stock preparation is necessary. |
| `READBACK-SOURCE`, `HEALTH-OWNER`, `FULL-CONTROL-OWNER` | Controller paths, sensor conversion and ownership | [Stock integration](stock-integration.md), [health procedure](procedures.md#read-direct-board-health) | Source inspection and recorded controller readings do not give electrical ratings. |
| `REGISTER-FINDINGS`, `POOL-TARGET-SOURCE`, `POW-SOURCE` | Register extraction, target conversion and host PoW | [Register maps](registers.md), [hash and target rules](mining.md#host-pow-byte-order) | The named source rules apply to the tested implementation. |
| `MINER-BINARY`, `PRODUCT`, `LAYOUT` | Matched 2.2.2 executable and configuration | [Command grammar](evidence/command-templates.json) | No binary or decompiler output is included. |
| `PACKETS`, `INIT-CAPTURE`, `CODEC` | Captured packets, decoded startup and offline codec | [Packets](evidence/packet-examples.json), [startup](evidence/initialization-capture.json) | Full source raw captures are in controlled storage. |
| `R002` through selected `R090` records | Individual physical test summaries | [59 selected run records](evidence/run-records.json) | Each quoted paragraph keeps its source line range and wording. |
| `R075-JOIN`, `SEGMENT-VECTORS` | Physical result comparison and frozen offline predictions | [Recorded sets](evidence/segmented-results.json), [expected sets](evidence/segment-vectors.json) | Full exhaustive enumeration is not repeated by this documentation check. |
| `CORE-CENSUS`, `CORE-MAP-RECEIPT` | Register, source and recorded result analysis | [Coordinate rule and exceptions](evidence/core-map.json) | No individual-core actuator or persistent counter. |
| `R089-NATIVE` | Native owner, direct health and accounting | [Control](evidence/run89-control.json), [213 transfers](evidence/run89-initialization.json) | Selected objects only. Process and access details are omitted. |
| `R089-STDOUT`, `R089-DECODE`, `R089-JOIN` | Native transfers, passive SPI and accepted-share association | [Recorded share packets](evidence/run89-share-packets.json), [association report](evidence/run89-physical-join.json) | Private recorded pool replies are identified by hash. |
| `R089-POW-INPUT`, `R089-POW-OUTPUT` | Unchanged offline verifier input and output | [Input](evidence/run89-pow-input.tsv), [output](evidence/run89-pow-output.tsv) | Local hash replay does not reproduce external acceptance. |
| `R075-ARCHIVE`, `R089-ARCHIVE`, `R090-ARCHIVE` | Lossless capture retention records | [Run 75](evidence/run75-capture-retention.json), [run 89](evidence/run89-capture-retention.json), [run 90](evidence/run90-capture-retention.json) | Raw storage paths and restore commands are omitted. |

The catalog also identifies the individual F04/F09/F10 audits and W92-W109
source searches. Quoted source records keep their source titles, terms and
provisional interpretations. The topic pages state the last accepted scope.

## Claim map

All physical claims use the single tested unit and stock board described above.
A firmware name identifies software configuration unless a separate physical
record shows it. Source inspection, offline replay, physical tests and external
replies are separate evidence types.

| Claim | Label and recorded observation | Sources and included data | Check method | Limit |
| --- | --- | --- | --- | --- |
| C01: identity | `proven` for recorded board markings, connector counts and firmware name | `COLD`, `CONTACTS`, `PRODUCT`, contact photo/annotation | Source and photograph inspection | Package marking, count and revision are unknown. |
| C02: transport | `proven` for selected captured signal roles. Mode interpretation is `strongly indicated`. | `R002`, `R003`, `INIT-CAPTURE` | Saved signal decode and byte comparison | Chip select and electrical margins are unmeasured. |
| C03: CRC | `proven` for the selected captured packet families | `PACKETS`, `TRANSPORT-RECEIPT` | Independent pair-reversal CRC and source-instruction checks | CRC-free and unused forms need separate interpretation. |
| C04: command grammar | `proven` for recorded forms, with source-only alternatives | `LAYOUT`, `COMMAND-RECEIPT`, `INIT-CAPTURE` | Table inspection and captured transfers | A compiled form is not automatically physically tested. |
| C05: initialization | `proven` after stock preparation | `R032`, `R052`, `R066`, `R089-NATIVE`, startup extracts | Full transfer order, count and admission CRCs | No independent supply startup. |
| C06: work and result fields | `proven` for selected work, nonce and coordinate fields | `PACKETS`, `CODEC`, `R078`, `R079`, run-89 share packets | Recorded byte decode and controlled physical contrasts | Nonzero trailing-byte meaning is unknown. |
| C07: local hash calculation | `proven` for reproduced saved result digests | `HASH-SUMMARY`, `R089-POW-INPUT`, `R089-POW-OUTPUT` | Offline canonical PoW replay | Does not prove each job or external acceptance. |
| C08: independent mining and acceptance | `proven` for run 89 after stock preparation | `R089`, `R089-NATIVE`, `R089-JOIN`, share packets | Physical TX/RX association, recorded-target replay and recorded external replies | Private replies are not included. No independent cold boot. |
| C09: register and reset behavior | `proven` for five selectors and selected masks/restore paths | `R037`, `R052`, `R066`, quoted run records | Directed reads, unchanged controls, changes and restoration with unchanged values | Unresponsive reset state has no readable defaults. |
| C10: relative clock effect | `proven` for selected timing. Clock-law interpretation is `strongly indicated`. | `R059`, `F09-CLOCK-PATH-AUDIT` | A/B/A/B/A, sibling controls and restoration | No absolute MHz, voltage or general safe tuning range. |
| C11: allocation and wrap | `proven` for the selected coordinate, byte and modulo contrasts | `R043`, `R044`, `R053`, `R077`-`R079`, core-map extract | Full frozen predictions and physical results | No physical anatomy or unlimited recurrence claim. |
| C12: retention and segmentation | `proven` for tested external retention and full segmented valid sets | `R068`-`R075`, vectors and recorded sets | Controlled drain/endpoint changes, set equality checks | Invalid emissions vary. Internal storage is unknown. |
| C13: reported cores and completion | `proven` for software-visible census and tested completion/rearm | `CORE-CENSUS`, `R075`, `R076`, firmware coverage | Source dataflow, CRC replies, coordinate coverage and event contrasts | Neither an individual-core control nor an emitted-result counter. |
| C14: fan control | `proven` for reversible two-fan speed and run-89 automatic cycle | `R083`, `R085`, `R087`, `R089`, control extract | Direct temperature, stable tachometer baseline, level readbacks and restoration | Independent fan control and general thermal policy are unproved. |
| C15: stop and cold stock return | `proven` for the specified run-89/run-90 sequence | `R089`, `R090`, capture retention records | Owner release, direct state, separate watchdog/meter and passive return capture | Historical last state. Warm return and rail discharge are unproved. |
| C16: firmware coverage | `proven` for named source identities and recorded offline outcomes | F01-F30 rows and individual catalog records | Matched files, selected firmware-instruction cases and source searches | Finite source scope does not show full silicon knowledge. |
| C17: failed gates | `proven` for the recorded failures and test stops | `R038`, `R039`, `R042`, `R045`, `R080`, `R081`, `R085`, `R087` | Separate run records and recorded failure observations | Later success does not change earlier failure status. |
| C18: sensor/ADC access | `proven` for selected raw responses and changes | `R038`, `R052`, `F05-HEALTH-CONTROL-AUDIT`, `W101` | A/B/A data, restoration with unchanged values and source conversion checks | Channel identity, scale and junction calibration are unknown. |
| C19: UART root access | `proven` for the tested J2 console on firmware 2.2.2 | `R001-UART`, `R001-ACCESS`, selected console records | Compare the recorded UID 0 reply and stock console startup line | No password prompt on this console. This does not show SSH access or replacement firmware recovery. |
| C20: J2 pads | `proven` for the visible pad shapes. Ground is an operator identification. TXD/RXD order is `unknown`. | `PHOTO-J2`, `OPERATOR-J2`, J2 detail and operator report | Photograph inspection and the operator's stated identification | No new continuity or signal measurement. |

## Included assets and rights

The [manifest](evidence/manifest.json) identifies each included asset and its hash.
Each entry states its source ID, derivation and redaction.
Source hashes and asset hashes are different fields in different catalogs.
A hash gives identity, not access or an independent reproduction.

The authored text and diagrams use the repository's CC BY 4.0 scope.
The three source photographs are unchanged. The assembly and reverse views have
the margins removed as recorded in the manifest. Labels and stickers remain visible.
The connector image has existing annotations. Its source identity and annotation
coordinates stay in the included record.
Public distribution still must have the owner's review of source rights and redactions.
No permission to redistribute manufacturer binaries is assumed.

The source raw captures stay in controlled research storage.
The archive records report successful lossless recovery checks at capture closeout.
This documentation change rechecks selected decoded data and local PoW.
It does not decompress and re-decode each historical raw capture.

## Gate failures and corrections

The [operation table](operation.md#faults-and-failed-gates) records the failed
warm returns, thermal stop, owner defects and cooling gates.
The [research page](research-notes.md) records omissions and invalid-result classes.
Important interpretation changes include:

1. Bank B has 20 contacts. The earlier 18-contact report was incorrect.
2. A readable 32-core count has recorded evidence. Individual-core control is still unproved.
3. Early incomplete result sets stay incomplete. Later segmentation recovered a different, fully predicted population.
4. Register-3 bit 16 has tested completion/rearm behavior. It is not a pending-result flag or persistent counter.
5. Run 89 passed automatic cooling with a stable baseline. Run 85 did not pass its specified baseline gate.

## Verification record

Reviewer: Codex. Date: 2026-10-08 (Australia/Sydney).
These checks use saved evidence. No new physical test was done.

| Check | Result and limit |
| --- | --- |
| Source identities and extracts | All 158 source files agree with their catalog hashes. The 375 quoted paragraphs, 39 research rows and 30 firmware rows agree with their sources. |
| Protocol and initialization | Twelve packet CRCs, ten command exchanges and all 162 recipe rows pass. Both startup records have thirty correct admission replies. |
| Mining replay | All 121 share digests and recorded target decisions agree. All 4,581 native transfers agree with the saved SPI decode. |
| Segmented results | All ten segments and 300 source sets agree. Replay gives the recorded digests for 475 valid identities and four invalid candidates. |
| UART | All four quoted console lines agree with their source records. Ground is an operator identification. TXD/RXD order is unknown. |
| Photographs and figures | The two cropped views keep the source pixels. All 28 SVG files render. Packet and register metadata cover each byte or bit. |
| Links, assets and repository checks | Local links and section anchors pass. All 87 asset hashes and 16 repository tests pass. The reference check and whitespace check pass. |
| English | Changed text was examined against the ASD-STE100 Issue 9 rules and dictionary on 2026-10-08. The identified wording and procedure errors are corrected. |

The format comparison used the BM2382 README, topic pages, hardware views and
figure inventory. The IEN616 README keeps the template section order and progress
table columns. Its root index entry opens the README. The topic pages keep
IEN616 findings and the tested KA Box conditions.

Rendered pages show figures beside their claims. Changed figure labels fit at their display size.
The comparison found no format problem. No known wording problem is open in the changed text.

The [validation summary](evidence/validation-summary.json) gives the check totals,
methods and verifier hashes. The [procedures](procedures.md#saved-evidence-checks)
give the steps for offline checks. The English source is
[ASD-STE100 Issue 9, 2025-01-15](https://www.asd-ste100.org/assets/files/ASD-STE100_ISSUE9.pdf).
The wording check was an agent review. No independent editorial review was done.

These checks do not repeat exhaustive nonce enumeration or the decoding of all
historical raw captures. Recorded pool replies stay private. Offline replay
does not repeat external acceptance or add a physical capability.
