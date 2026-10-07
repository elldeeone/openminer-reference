# IEN616 task procedures

These tasks describe the tested stock-prepared board. A new powered test needs
its own approved procedure, operator, cooling and watchdog conditions.
The saved-evidence tasks at the end need no device access.

## Initialize and admit the stock board

**Purpose:** get the tested thirty-address state before independent work.

1. Complete stock preparation and the [ownership and health gates](operation.md#ownership-and-tested-operating-state).
2. Check the SPI mode, word width and initial host speed.
3. Apply the GPIO schedule in [initialization](initialization.md#ordered-native-recipe).
4. Send the 162 [recipe rows](evidence/initialization.tsv) in order.
5. Keep each row's length, speed, zero padding and delay after its transfer.
6. Make sure that the count at row 0 is 30.
7. Make sure that all thirty admission replies at rows 130-159 pass their checks.
8. Send the known work.
9. Complete its fifty polls.
10. Use the recorded work inputs and host PoW to examine each returned nonce.

The native owner checks direct health during the sequence and long waits.
A failed gate stops startup. Partial admission is not a usable chain.
A passed fixed-work check is separate from a new pool-accepted share.

## Read a selected register

**Purpose:** read one supported register at an identified logical address.

1. Select an address from 1 through 30 and a supported [register](registers.md#register-index).
2. Build `A53C 0ANN SELECTOR_BE[2]` with that address and selector.
3. Fill the transfer with zero to `14 + 6 × address` bytes.
4. Use the tested 1 MHz schedule after startup.
5. Find a full 12-byte `1ANN` reply in the received data.
6. Check its CRC, requested address and requested selector.
7. Read bytes 6-9 as one big-endian value.
8. Keep the full value and the work context in the record.

For example, address 1/register 5 uses `A53C0A010005` in a 20-byte transfer.
The [captured exchange](evidence/command-examples.json) returns
`A53C1A0100050000000136DA` during early startup.
The value is 1 in that specific state.

**Failure:** reject missing, truncated, mismatched or CRC-invalid replies.
No reply is not a zero value. A read in a new state needs separate qualification.

## Read the reported core count

**Purpose:** check the stock-reported count without a physical-core assumption.

1. Complete the full initializer and its admission gates.
2. Read register 3 at each of the thirty addresses.
3. Validate each register reply.
4. Read the low byte from each returned value.
5. Compare the thirty counts with the recorded value 32.

The selected stock code reports 960 cores in total.
This does not identify physical packages, lanes or individual-core controls.

## Service work and results

1. Keep the current job, target, hash, timestamp and nonzero slot together.
2. Send the tested control `A53C040000E5` before the new work packet.
3. Encode the 70-byte [work packet](protocol.md#work-packet) in its 672-byte transfer.
4. Use the tested poll request in a 618-byte transfer.
5. Distinguish the four-byte empty echo from a 16-byte result.
6. Check the result CRC, slot, logical address and trailing byte.
7. Associate the result with the preceding assigned work and current job.
8. Calculate host PoW.
9. Compare the digest with the recorded pool target.
10. Reject invalid, stale, unsupported and duplicate results.
11. Associate each submitted result with its request ID and pool reply.

The run-89 native poll is `A53C0800FFFF`.
The [mining example](mining.md#one-recorded-example) gives full work/result bytes,
canonical inputs, digest and target. Register-3 bit 16 is not a pending-result flag.

## Read direct board health

1. Read both `/sys/class/fans/fan0/rpm` and `/sys/class/fans/fan1/rpm`.
2. Read the TMP112 configuration and temperature on bus 0 at address `0x48`.
3. Byte-swap both returned SMBus words.
4. Check the configuration mode before temperature conversion.
5. For the recorded normal mode, read temperature bits 4-15 as a signed 12-bit integer.
6. Multiply this integer by 0.0625 °C.
7. Apply the temperature, fan-speed and freshness gates in [operation](operation.md).

Run 89 records configuration `A060` in SMBus order, or `60A0` after the byte swap.
Configuration bit 4 is zero for normal mode.
Its first `1024` temperature sample becomes `2410`, which gives 36.0625 °C.
The source also supports extended mode with bits 3-15.
The independent owner rejects mode/data disagreement and nonpositive or excessive temperatures.
Sources: `HEALTH-OWNER`, `F05-HEALTH-CONTROL-AUDIT` and `R089-NATIVE`.

A sensor error is not a temperature. Chip sensor messages and register 7 do
not replace this direct board-health path.
The automatic fan cycle needs the stable baseline and level readbacks in
[two-fan control](stock-integration.md#two-fan-control).

## Stop and close the operating window

1. Stop new work.
2. Release the independent SPI owner.
3. Apply the tested enable/reset LOW actions.
4. Make sure that both controller output readbacks are LOW.
5. Restore both fan-level readbacks to 100.
6. Complete the direct-health and ownership closeout checks.
7. Complete the separate watchdog OFF check.
8. Record the relay state and meter values.

The documented result ends at OFF / 0 W / 0 A.
It does not prove internal rail discharge.
The later cold stock return is a separate test. General warm return is unproved.

## Saved-evidence checks

The tasks below check the included records without device access.
Executable verification tools stay in their own repositories.

## Check an asset

1. Select the file in [the manifest](evidence/manifest.json).
2. Calculate its SHA-256 value with an independent hash tool.
3. Compare the result with the manifest value.
4. Read its source ID, derivation and redaction fields.
5. Find the source identity in [the source catalog](evidence/source-catalog.json).

A matching hash proves byte identity. It does not prove a technical claim or
make a private source available.

## Check a packet

1. Select a packet in [the packet extract](evidence/packet-examples.json).
2. Check its header and length against [the interface table](protocol.md).
3. Read its command as a big-endian 16-bit word.
4. Calculate the CRC with the documented pair-reversal rule.
5. Compare the calculated CRC with the last two packet bytes.
6. Decode each field with the offset, width and byte order from the figure metadata.

The extract has twelve CRC packets. Work packets have 70 bytes.
Result packets have 16 bytes. Register writes and replies have 12 bytes.
Transfer padding is outside those packet lengths.

## Check initialization and admission

1. Open [the stock capture](evidence/initialization-capture.json).
2. Check all 162 transfer lengths against their TX and RX byte counts.
3. Compare the command order with [the initializer](initialization.md).
4. Examine the addressed register-3 replies at positions 1 through 30.
5. Make sure that all thirty admission replies have correct CRCs and the value `12100020`.
6. Open [the native record](evidence/run89-initialization.json).
7. Check its 213 transfers and its stated speeds.
8. Keep differences between stock settings and the independent recipe explicit.

The native recipe can use different selected clock words from the recorded
stock capture. A byte difference is not automatically an error.
The source identities and tested setting must show the cause.

## Replay the accepted-share examples

1. Read [the TSV input](evidence/run89-pow-input.tsv) as six tab-separated fields per row.
2. Convert the first four decimal u64 words to little-endian bytes.
3. Concatenate those words to form the recorded 32-byte pre-PoW hash.
4. Read the decimal timestamp and hexadecimal nonce from the remaining fields.
5. Calculate Kaspa PoW with an independently checked implementation.
6. Compare the last little-endian digest with [the saved output](evidence/run89-pow-output.tsv).
7. Find the corresponding nonce and recorded difficulty in [the association report](evidence/run89-physical-join.json).
8. Apply the recorded pool target parser.
9. Compare that target with an independently calculated target.

There are 121 rows. The row order matches the report's accepted-share order.
The research example `kabox_pow_stages` reads this six-field input from standard
input. It prints the row number, intermediate hashes and last digest.
The separate `ks5_recorded_pow` example accepts an optional seventh field for
the recorded decimal difficulty. It prints the digest and target decision.
The two sources stay in the research repository.

This replay checks local PoW. The external acceptance claim also depends on
the private recorded replies and their published association report.

## Check the segmented population

1. Read [the ten frozen vectors](evidence/segment-vectors.json).
2. Check that their offset ranges cover 0 through `2^36 - 1` without overlap.
3. Make sure that each start is divisible by 64.
4. Make sure that each end is 63 modulo 64.
5. Check that no source has more than four expected valid identities per segment.
6. Compare each expected set with [the physical comparison](evidence/segmented-results.json).
7. Keep invalid candidates in a separate result class.
8. Make sure that there are 475 distinct valid identities, with zero missing or extra valid identities.

Recalculation of the 475 supplied candidate hashes is a small offline check.
Independent exhaustive enumeration is a different check. The source record
keeps its enumeration count and source identities.

## Requirements for a new physical check

WARNING: Do not reuse a consumed powered command. The recorded commands have
one-use identities and apply only to their specified accepted conditions.

A new procedure must have the tested unit identity, a responsible operator and
approved instrument connections. It must define ownership, cooling, direct
health, capture, finite duration, failure response and restoration before power.

The record must keep unchanged work inputs, raw observations, source hashes,
failed gates and last power state. A new-board or independent cold-boot test
also must have the missing electrical evidence in [unknowns](unknowns.md).
