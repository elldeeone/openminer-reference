# BM2382 task procedures

These procedures describe the tested KS5 Pro configuration. They do not
replace power, reset, cooling, watchdog or exclusive-owner requirements.
Use the [complete initializer](initialization.md) for cold startup.
Use [stock integration](stock-integration.md) for the supply discharge, power
qualification, reset schedule and UART mapping.

## Enumerate the assigned chips

**Purpose:** confirm the assigned selector inventory on each tested chain.

1. Complete the inactive setup and address writes in the initializer.
2. Send the [identity poll](protocol.md#register-poll) in its recorded phase:
   `55aa520500000a`.
3. Require complete [identity frames](protocol.md#bm2382-identity-response).
   Check CRC5, family `2382`, register `00` and the repeated selector.
4. After assignment, require selectors `00,02,…,b6`: 92 positions per chain.
5. Complete the full startup gate before declaring the chain ready.

The complete address gate contains three 92-frame groups. The first group
has zero selectors; the later groups contain the assigned selectors. These
are separate startup observations. Preserve their order. Use the
[276-frame fixture](evidence/address-responses.hex) and the
[response-gate criteria](initialization.md#response-gates).

**Failure:** an incomplete, invalid or unexpected inventory fails startup.
Use the qualified stop/restore path. Do not continue with a partial inventory.

## Read a selected register

**Purpose:** obtain a qualified readback from a tested ASIC or core target.

1. Use a register and operating state covered by the retained observations.
   Identify the ASIC selector. Identify the core selector for a core request.
2. For an ASIC read, build the `42` request in
   [directed register access](protocol.md#directed-register-access).
   For a core read, build its `45` request. Calculate the request CRC5.
3. Send one request within the qualified controller schedule. Preserve the
   response deadline and the recorded request/response context.
4. Require one complete response with a valid CRC5 and the requested ASIC
   selector and register. A core response must also match the core selector
   and the observed type flags `40`.
5. Decode an ASIC value from bytes 3–6. Decode a core value from bytes 4–6.
   Use most significant byte first. A missing response is not a zero value.

The typed core response is defined [here](protocol.md#typed-core-response).
Do not treat every read as passive. Counter reads affected the output/count
relationship in matched experiments. Use the
[bounded read policy](research-notes.md#counters-and-read-effects).
No universal safe polling interval has been established.

**Failure:** reject a truncated, invalid, stale or mismatched response.
Do not change an unrelated register to obtain a response.

## Read temperature

**Purpose:** obtain stock-derived temperature readings from register `c4`.

1. Write [ASIC c0](registers.md#0xc0-temperature-access-setup) with `11000000`.
2. Apply the scheduled wait. Write c0 with `11020000`.
3. Apply the scheduled wait. Poll [ASIC c4](registers.md#0xc4-temperature-result).
4. Require complete [temperature responses](protocol.md#temperature-response).
   Check CRC5, selector, register `c4`, observed validity bit and freshness.
5. Extract the low 16-bit ADC code. Apply the stock conversion:

```text
temperature_c = ((adc_code - 0.5) * 662.88 / 4096.0) - 287.48
```

Exact packet sequence:

```text
55aa510900c0110000001f
55aa510900c0110200000a
55aa520500c417
```

Use the waits and full-inventory gates in the initializer during startup.
The historical mining capture placed approximately 10 ms between each write
and the next packet. This is an observation, not a minimum silicon timing.
Keep temperature service within the qualified controller schedule.

**Limit:** these are stock-derived readings. External sensor calibration is
unknown. A zero or stale response is not a valid temperature measurement.

## Change to the supported fast UART setting

**Purpose:** complete the qualified low-speed to fast-speed transition.

1. Complete the ramp, temperature cycles and hold in
   [initialization](initialization.md#replay-order).
2. Send row 340 to chain 0 while it is still at low speed:
   `55aa510900600000010015`.
3. Configure that host UART for 1,500,000 baud, 8N1. Read back its settings.
4. Repeat the packet, host configuration and readback for chain 2.
5. Apply the documented mining setup and first-work wait. Start bounded work.
6. Require valid subsequent traffic before accepting results. Apply the
   [result-binding checks](mining.md#bind-and-validate-a-result).

The packet writes [ASIC 60](registers.md#0x60-uart-configuration) with
`00000100`. The captured fast wire decodes at 1,562,500 baud. The host setting
and physical decoded rate are different quantities.

**Failure:** a failed host readback fails startup. Use the qualified stop/restore
path. Do not try unrelated divisor values to force the transition.

## Follow the supported mining lifecycle

1. Initialize and validate both chains.
2. Build work from the current pool job. Use the supported dispatch policy.
3. Bind each returned result to recent current work. Calculate host PoW.
4. Submit only target-valid, current and nonduplicate candidates.
5. On Stop or fatal fault, use the qualified terminal-state and restoration path.

See [mining](mining.md) for packet and target checks. See
[operation](operation.md#required-control-sequence) for stop and recovery.
Diagnostic counters do not provide share credit.

[BM2382 overview](README.md) · [Protocol](protocol.md) · [Evidence](evidence.md)
