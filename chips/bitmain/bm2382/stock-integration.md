# KS5 Pro stock integration

This page describes the stock controller interface used by the tested driver.
The miner has three installed hashboards. Two hashboards had data connections
and mined on chains 0 and 2. All three remained connected to the shared supply.
The site power limit prevented three-hashboard mining.

The tables below describe the inspected driver implementation. Successful
stock-board runs support the complete integration. These values are not chip
pin assignments, measured chip rail voltages or manufacturer ratings.
Do not use selected steps as a substitute for the complete startup sequence.

## Controller resources

| Resource | Tested interface |
| --- | --- |
| Chain 0 UART | `/dev/ttyS1` |
| Chain 2 UART | `/dev/ttyS3` |
| Chain 0 presence / reset | GPIO 426 / 427 |
| Chain 2 presence / reset | GPIO 430 / 431 |
| Required presence inputs | 426 high, 428 low, 430 high |
| APW17 output enable | GPIO 412 |
| APW17 GPIO-I2C clock / data | GPIO 459 / 461 |
| Fan PWM outputs | `/sys/class/pwm/pwmchip8/pwm0` and `pwm1` |
| Four fan tachometers | `/sys/class/pwm/pwmchip12/pwm0` through `pwm3` |
| PCB temperature bus | `/dev/i2c-0`; sensor addresses `48`, `4c`, `4a`, `4e` hexadecimal |

A low presence input means that the tested data path is absent. It does not
measure whether a hashboard has supply power. The table does not identify the
physical socket of the third installed board.

The fixed analyzer tap responds to chain 2's UART path. Its exact physical
socket position has not been independently verified. Use logical chain names.

## Admission order

1. Acquire exclusive control. Keep the stock mining process and other writers
   from using the UART, PSU, reset or fan interfaces.
2. Admit the stock PWM platform. Require the `cv183x_pwm` and `cv183x_base`
   modules, the PWM outputs and all four tachometer inputs.
3. Set both fan PWM outputs to 100%. Verify the applied values.
4. Check the three presence inputs in the resource table.
5. Require all four fan tachometers to report at least 6,300 RPM.
6. Complete the APW17 supply sequence below.
7. Complete the board reset sequence below.
8. Open both UARTs at 115,200 baud, raw 8N1. Check host settings.
9. Execute [ASIC initialization](initialization.md), including both chains'
   identity and temperature gates. Then start bounded mining and supervision.

Use monotonic deadlines. Keep timing-sensitive execution on the controller.
Fail admission on a missing interface, failed readback or failed response gate.
Use the terminal sequence after failure.

### Stock PWM platform

The inspected platform sets these controller pinmux words. Each write requires
exact 32-bit readback. These are stock controller addresses, not ASIC registers.

| Address | Value |
| --- | --- |
| `030010e4` | `00000001` |
| `030010d0` | `00000001` |
| `030010a8` | `00000002` |
| `030010ac` | `00000002` |
| `030010b0` | `00000002` |
| `030010b4` | `00000002` |

Export the two output channels and four tachometer channels as needed.
Configure each tachometer with `period=1000000` ns and `enable=1`.
Require readable capture values and writable output period, duty and enable.
The implementation bounds root discovery at 100 checks, 10 ms apart. It bounds
channel setup and field writes at 20 attempts, 50 ms apart.

## APW17 supply sequence

GPIO 412 controls the stock supply output enable. This page describes APW17
control and firmware-derived voltage feedback. It does not specify chip rails.

| Step | Action | Gate or wait |
| --- | --- | --- |
| 1 | Read version, command `02` | Payload u16 LE must equal `00c1` |
| 2 | Disable watchdog, command `81`, payload `00 00` | Reply u16 LE must equal 0 |
| 3 | Drive GPIO 412 low | Wait at least 61,000 ms with output disabled |
| 4 | Read voltage feedback, command `04` | Proceed at ≤4 V, or after two consecutive samples at ≤10 V |
| 5 | Read firmware, command `01` | Payload u16 LE must equal `1e08` |
| 6 | Send DA command `83`, payload `15 00` | Require a valid reply |
| 7 | Read status, command `0a` | Payload u32 LE must equal 0 |
| 8 | Drive GPIO 412 high | Wait 1,000 ms |
| 9 | Read voltage feedback, command `04` | Require 13.5–16.5 V in the stock feedback conversion |
| 10 | Enable watchdog, command `81`, payload `01 00` | Reply u16 LE must equal 1 |

For step 4, multiply the returned u16 LE value by `0.003965` to obtain the
firmware-derived voltage. Fail after 21 unacceptable samples. Wait 1,000 ms
between retries. A high sample resets the consecutive ≤10 V count.

For step 7, fail after 20 nonzero statuses. Wait 3,000 ms between retries.
For step 9, fail after three out-of-range samples. Wait 1,000 ms between retries.
Do not retry a transport fault as an acceptable voltage or status reading.

The 61 s interval is output-off discharge time. It is not an energized
supply-settle interval. Feedback values have not been independently calibrated.
The DA payload `15 00` is decimal 21 as a u16 LE value. The stock miner's
command value `1500` belongs to a different control interface. Neither value
is a measurement of chip millivolts.

### APW17 frame format

Requests and replies use:

`55 aa COUNT COMMAND PAYLOAD SUM_LO SUM_HI`

`COUNT` equals payload length plus 4. Total frame length equals `COUNT+2`.
The checksum is the unsigned 16-bit sum of `COUNT`, `COMMAND` and all payload
bytes. Store it in little-endian order. This checksum is not a CRC.

Require the header, exact frame length, matching command and checksum.
Commands `01`, `02`, `04`, `81` and `83` expect 8-byte replies. Command `0a`
expects a 14-byte reply. Retry a complete invalid reply at most three times
in total. Leave the command state unchanged between these attempts.

### GPIO-I2C transport

The stock adapter transmits each frame byte in a separate I2C transaction.
For a write, send address byte `20`, bridge byte `11` and the frame byte.
For a read, send address byte `21` and receive one frame byte. The 7-bit
address is `10` hexadecimal. Send bits most significant first.

Initialize SCL and SDA high. A start drives SDA low while SCL is high.
A stop drives SDA high while SCL is high. Change SDA direction to input for
slave ACK and received data. Bound ACK polling to four checks.

Use these edge steps for the GPIO transport. Each `wait` is 1 ms.

| Operation | Repeated edge steps |
| --- | --- |
| Start | SDA high; SCL high; wait; SDA low; wait |
| Send each bit | SCL low; wait; set SDA to the bit; SCL high; wait |
| ACK | SCL low; wait; set SDA input; read SDA. If low, SCL high; wait; accept. If high, clock high/wait/low/wait before another check. Fail after four checks. |
| Receive each bit | Set SDA input; SCL low; wait; sample SDA; SCL high; wait |
| End received byte | SCL low; wait; set SDA output; SDA high; SCL high; wait |
| Stop | Set SDA output; SCL low; wait; SDA low; SCL high; wait; SDA high; wait |

These steps transcribe the tested GPIO adapter, including its sampling phase.
They do not specify a generic I2C driver. After all request bytes,
wait 400 ms. Read the expected reply length, then wait 100 ms. These are
controller transport waits, not chip timing requirements. The per-byte
start/stop behaviour is part of this adapter. Do not replace it with one
continuous frame transaction without separate qualification.

## Board reset sequence

Prepare GPIOs 427 and 431 as outputs. Wait 100 ms. Apply these levels to both
outputs in order:

| Step | Level | Wait before next step |
| --- | --- | --- |
| 1 | High | 100 ms |
| 2 | Low | 100 ms |
| 3 | High | 100 ms |
| 4 | Low | 100 ms |
| 5 | High | Check both readbacks |

The driver then uses the waits in [ASIC initialization](initialization.md).
A historical stock cable capture had about 4.51 s between final reset release
and first low-speed UART traffic. The supported driver waits 4.7 s before each
chain's initialization. Keep the captured observation and driver wait separate.

These are controller levels and software delays. The BM2382 package reset pin,
electrical polarity and pulse requirements remain unknown.

## Cooling and operating supervision

Fan PWM uses a 100,000 ns period. Duty is `period × percent / 100`.
Set `enable=1` on both output channels. Require exact readback of all fields.
Bound write retries to 20 attempts, 50 ms apart. A reported write error does not
prove that the hardware value stayed unchanged; read back the applied value.

Convert each positive tach capture value to RPM with
`30000000000 / capture`. Require four tachometers. Full-speed admission allows
120 checks at 500 ms intervals. It reapplies full speed every 20 failed checks.
During operation, the adapter requires at least 400 RPM on each channel.

The stock-derived fan policy clamps PWM to 15–100%. Its base target is 80 °C,
with a PCB-temperature adjustment. It uses a trimmed ASIC-temperature average
and a hottest-chip correction. The adapter seeds the control state at its
steady stage. It then updates at 15 s intervals. The configuration also defines
a 5 s startup stage with 21 updates; this adapter's cold seed skips that stage.
Its full-speed override is 99 °C.
These values describe this implementation. They are not absolute chip limits.
A new board requires its own qualified thermal control.

Startup checks 92 temperature frames on each chain. During mining, the tested
fan policy uses ASIC telemetry from chain 2. It requests that chain at 1 s
intervals and requires a complete 92-position collection. It also reads all
four PCB sensors. It does not continuously collect all ASIC temperatures from
both chains. The PCB reader uses register 0, reads two bytes and interprets the
first byte as a signed integer temperature. It rejects values outside −40 to
125 °C. That range is a parser limit, not a supported operating range.

### Applied fan calculation

Convert each ADC value with `(adc−0.5) × 662.88 / 4096 − 287.48`.
Round to the nearest integer degree, with half-way values away from zero.
Sort the 92 values. Reject a hottest-to-coldest difference greater than 40 °C.
Remove the three lowest and three highest values. Divide the sum of the other
86 values by 86 with integer truncation. Call this average `A` and the hottest
value `H`. The measured control temperature is:

`M = A + 2 × max(H−92,0) + 2 × max(H−97,0)`

Read all four PCB sensors. Use their minimum temperature `P` to set the target:

| PCB minimum `P` | Target `T`, °C |
| --- | --- |
| ≤47 | `75 − min(48−P,16)/1.5` |
| 48 | 76 |
| 49–52 | 77 |
| 53 | 76 |
| ≥54 | 75 |

On the first valid sample, retain the current PWM and seed both prior errors
with `T−M`. Start the steady update clock at that sample. At each 15 s update,
set `E=T−M`. The steady delta is `truncate(−0.75×E − (E−previous E))`.
Truncate toward zero. Add this delta to the current PWM and clamp to 15–100%.
Retain the new error and update time. The derivative coefficient is zero.

The adapter applies the 99 °C full-speed override before the normal update
interval check. It checks all four tachometers 2 s after a PWM change.
These calculations reproduce the inspected stock policy. They do not qualify
another board's sensor, heatsink or safe temperature.

### Supply supervision

Supply supervision reads voltage and status, with a 10 s wait between cycles.
It rejects invalid transport replies. Voltage failures accumulate until a good
reading resets their count; nonzero statuses accumulate. The adapter faults
when voltage failures exceed three, or when status is nonzero and either its
count exceeds three or that cycle also has a voltage failure.
Cooling, supply and lease supervision remain active during bounded operation.
Observed session temperatures and power are in [operation](operation.md).

## Terminal sequence

1. Stop work dispatch and cancel UART activity.
2. Command full-speed cooling.
3. Drive supply output enable GPIO 412 low. Check its readback.
4. Drive reset GPIOs 427 and 431 low. Check both readbacks.
5. Record all three action results. Attempt every action even if an earlier
   action failed.
6. Confirm the required terminal state before restoration. Restore UART and
   stock service ownership through the tested recovery path. Use its cooling
   park before final relay power-off.

A low GPIO confirms the controller output state. It does not measure chip rail
voltage or instantaneous discharge. The final relay OFF and wall 0 W / 0 A
observation is separate evidence.

Pool EOF and normal Stop were physically tested. The included evidence does
not demonstrate cutoff after every Linux, SoC or safety-owner failure.
The APW17 watchdog command is not proof of a complete independent shutdown
mechanism. See [remaining qualifications](unknowns.md#remaining-driver-qualifications).

## Evidence boundary

This page transcribes the inspected stock adapters at the source revision in
[evidence](evidence.md). The complete integration produced accepted shares and
the documented recovery result. A successful integration run does not establish
each feedback threshold as a separately measured electrical specification.
Chip-level design facts remain in [new-board requirements](hardware.md#new-board-requirements).

[BM2382 overview](README.md) · [Initialization](initialization.md) · [Operation](operation.md)
