# IEN616 figures

Chip diagrams describe the tested IEN616 interface. Board diagrams identify the KA Box setup.
All diagrams are editable SVG files. Each matching JSON file identifies the
setup, claims and sources. The [hardware page](../hardware.md#board-views) also
includes three board views and the existing connector annotation.

Packet diagrams number every byte from the first `A5` byte.
Register diagrams number every bit, with bit 0 at the right.
Blue identifies a transport or source field. Amber identifies a selected
physical contrast. Hatching marks unknown meaning. The fan chart has its own legend.

Packet metadata gives each field's offset, width, unit and byte order.
It also gives CRC coverage, or a null checksum for a form without CRC.
Register metadata gives bit offsets and widths. Unknown fields are not zero defaults.

## Boards and operating sequence

| Figure | Claims and sources | Scope |
| --- | --- | --- |
| [F01: System boundaries](system.svg), [metadata](system.json) | C01/C02/C14/C15. `COLD`, `CONTACTS`, `R002`, `R003`, `R089`, `R090` | Controller, board, console and observed control paths. |
| [F08: Capture connections](capture-connections.svg), [metadata](capture-connections.json) | C01/C02. `COLD`, `CONTACTS`, `R002`, `R003` | Local contact names and analyzer channels. No inferred package pinout. |
| [F04: Startup and stop gates](lifecycle.svg), [metadata](lifecycle.json) | C05/C15. `R052`, `R066`, `R089`, `R090` | Stock preparation, admission, operation and failure paths. |
| [F25: Ordered initialization](initialization-sequence.svg), [metadata](initialization-sequence.json) | C05. `RECIPE-DATA`, `STARTUP-OWNER`, `R089-NATIVE` | Transfer indexes, SPI speeds, waits and admission replies. |
| [F26: Reset and recovery](reset-recovery.svg), [metadata](reset-recovery.json) | C09/C15. `R052`, `R066` | Separate enable/reset tests and full-initializer recovery. |
| [F27: Two-fan response](fan-response.svg), [metadata](fan-response.json) | C14. `R089`, `R089-NATIVE` | Recorded median RPM at the three tested control stages. |

## Commands and registers

| Figure | Claims and sources | Scope |
| --- | --- | --- |
| [F09: Command word](command-word.svg), [metadata](command-word.json) | C04/C06. `CODEC`, `PACKETS` | Work/result bit positions, logical address and slot. |
| [F14: Count request](count-request.svg), [metadata](count-request.json) | C03/C04/C06. `LAYOUT`, `R089-NATIVE` | Six-byte request without CRC. |
| [F15: Count reply](count-reply.svg), [metadata](count-reply.json) | C03/C04/C06. `LAYOUT`, `R089-NATIVE` | Recorded logical count. Physical package count is unknown. |
| [F10: Register write](register-write.svg), [metadata](register-write.json) | C03/C04/C06. `LAYOUT`, `PACKETS` | Selector, value and CRC coverage. |
| [F11: Register read](register-read.svg), [metadata](register-read.json) | C03/C04/C06. `LAYOUT`, `R089-NATIVE` | Request selector. Padding depends on the logical address. |
| [F06: Register reply](register-packet.svg), [metadata](register-packet.json) | C03/C09. `PACKETS`, `COMMAND-RECEIPT` | Returned selector, 32-bit value and CRC coverage. |
| [F16: Startup control](startup-control.svg), [metadata](startup-control.json) | C03/C04/C06. `LAYOUT`, `R089-NATIVE` | Four-byte body for the recorded 01, 0B and 03 forms. |
| [F17: Work control](work-control.svg), [metadata](work-control.json) | C03/C04/C06. `LAYOUT`, `R089-NATIVE` | Six-byte 04 form before the selected work sequence. |
| [F12: Result poll](result-poll.svg), [metadata](result-poll.json) | C03/C04/C06. `LAYOUT`, `R089-NATIVE` | Native FFFF data word. Source-template 0000 is separate. |
| [F13: Empty result echo](empty-reply.svg), [metadata](empty-reply.json) | C03/C04/C06. `LAYOUT`, `R089-NATIVE` | Four-byte echo without nonce or CRC. |
| [F18: Sensor request](sensor-request.svg), [metadata](sensor-request.json) | C03/C04/C06. `LAYOUT`, `F05-HEALTH-CONTROL-AUDIT` | Fourteen-byte request without CRC. |
| [F19: Sensor reply](sensor-reply.svg), [metadata](sensor-reply.json) | C03/C04/C06. `LAYOUT`, `F05-HEALTH-CONTROL-AUDIT` | Five used sensor words, twenty unknown bytes and CRC. |
| [F20: Register 0](register-0.svg), [metadata](register-0.json) | C09/C10. `R052`, `R059`, `F09-CLOCK-PATH-AUDIT` | Source apply bit and physically tested bit-16 contrast. |
| [F21: Register 3](register-3.svg), [metadata](register-3.json) | C09/C13. `R037`, `R076`, `F02-COMMAND-REGISTER-AUDIT` | Reported count, work ID, completion and source control. |
| [F22: Register 5](register-5.svg), [metadata](register-5.json) | C09. `R052`, `F02-COMMAND-REGISTER-AUDIT` | Source low-nibble update. Other fields are unknown. |
| [F23: Register 6](register-6.svg), [metadata](register-6.json) | C09/C18. `R038`, `R052`, `F02-COMMAND-REGISTER-AUDIT` | Tested start bit and source mode field. |
| [F24: Register 7](register-7.svg), [metadata](register-7.json) | C09/C18. `REGISTER-FINDINGS`, `R038`, `R052` | Source low-12-bit extraction. No calibrated temperature. |

## Work and results

| Figure | Claims and sources | Scope |
| --- | --- | --- |
| [F02: Work packet](work-packet.svg), [metadata](work-packet.json) | C03/C04/C06. `PACKETS`, `CODEC` | All 70 bytes, field order and CRC coverage. |
| [F03: Result packet](result-packet.svg), [metadata](result-packet.json) | C03/C04/C06. `PACKETS`, `CODEC` | All 16 bytes, core coordinate and unknown trailing-byte meaning. |
| [F05: Work and pool decisions](mining-flow.svg), [metadata](mining-flow.json) | C06/C07/C08. `R089-NATIVE`, `R089-JOIN`, `R089-POW-INPUT`, `R089-POW-OUTPUT` | Host validation and recorded external acceptance are separate. |
| [F28: Allocation example](result-allocation.svg), [metadata](result-allocation.json) | C06/C11. `R089-JOIN`, `CORE-MAP-RECEIPT`, `R078`, `R079` | Recorded work start, logical source start, nonce and core coordinate. |
| [F07: Result retention](retention.svg), [metadata](retention.json) | C12. `R070`, `R071`, `R075`, `SEGMENT-VECTORS`, `R075-JOIN` | Observed result capacity and segmented recovery. No internal circuit claim. |

## Source photographs

| View | Claims and sources | Limit |
| --- | --- | --- |
| [Controller](../evidence/board-controller.jpg) | C01. `PHOTO-CONTROLLER`, `PHOTO-AUDIT`, `COLD` | The controller processor is not the mining ASIC. |
| [Assembly](../evidence/board-assembly-cropped.png) | C01. `PHOTO-ASSEMBLY`, `PHOTO-AUDIT`, `COLD` | Heatsinks cover the ASIC packages. |
| [Reverse](../evidence/board-reverse-cropped.png) | C01. `PHOTO-REVERSE`, `PHOTO-AUDIT`, `COLD` | Visible traces do not give a full net map. |
| [J2 pads](../evidence/j2-detail.png) | C20. `PHOTO-J2`, `OPERATOR-J2` | Square ground pad identified by the operator. TXD/RXD order is unconfirmed. |
| [Connector annotation](../evidence/connector-contacts.png) | C01. `CONTACT-PHOTO`, `CONTACT-RECEIPT` | 18/20 contacts. View coordinates are not manufacturer pin numbers. |

The [manifest](../evidence/manifest.json) contains each asset hash.
The [verification record](../evidence.md#verification-record) gives the visual
inspection and source checks. Board photographs have visible labels and stickers.
The owner must check rights and redactions before public release.
