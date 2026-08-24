## Industrial Conveyor Control System
This is a PLC-controlled conveyor system that is built using Ladder Logic (LD), it features Auto/Manual operating modes, product counting, and automatic jam detection with alarm/fault latching.
# Video Demonstration
The video demo showcasing the logic and simulation can be accessed here https://youtu.be/MfTsW2H3e5U

## Overview
This project implements a realstic conveyor control system with a motor that actuals the moving belt platform, circuit with emergency stops, auto and hand control, entry/exit photoelectric sensor logic for the auto-cylcing, and product counting with a hardware style counter.

## Features

- **Start/Stop/E-Stop control** — sealed-in `RunCommand` latch, hard-wired
  style Stop/EStop interlock
- **Manual mode** — direct motor run control via `ManualRun`
- **Auto mode** — `AutoCycle` gated by Entry and Exit sensors, driving
  `AutoRun`
- **Product counting** — `CTU` (count-up) counter incremented on the rising
  edge of the Exit sensor (via `R_TRIG`), preset value of 100, with a
  dedicated reset input
- **Jam detection** — `TON` timer (5s) armed while `AutoCycle` is active and
  neither Entry nor Exit sensor is triggered; on timeout, latches a
  `JamAlarm` (SET) and stops the auto run via the motor logic
- **Fault reset** — dedicated `ResetFault` input resets (`RESET`) the
  `JamAlarm` latch and the exit-cycle logic

## How It Works

1. `StartButton` sets `RunCommand`; `StopButton` or `EmergencyStop` breaks
   it.
2. In **Manual mode**, `RunCommand` drives `ManualRun` directly, which
   (provided no jam alarm is active) drives the `Motor`.
3. In **Auto mode**, product passing the `EntrySensor` sets `AutoCycle`;
   product clearing the `ExitSensor` resets it — this window represents one
   conveyor cycle. `AutoCycle` combined with `AutoMode` drives `AutoRun` →
   `Motor`.
4. Each rising edge on `ExitSensor` increments the `CTU` product counter
   (`ProductCount`), up to a preset of 100, resettable via `ResetCounter`.
5. If `AutoCycle` is active but neither sensor is triggering (i.e. product
   is stuck) for longer than **5 seconds**, the `JamTimer` (`TON`) times out
   and **latches** `GVL_IO.JamAlarm`, which interlocks the motor off.
6. The alarm can only be cleared via the dedicated `GVL_IO.ResetFault`
   input, which resets the `JamAlarm` latch.

  ## Skills Demonstrated

Ladder Logic programming, motor control interlocking (Start/Stop/E-Stop),
edge detection (`R_TRIG`), counters (`CTU`), timers (`TON`), latched
alarm/fault handling, auto/manual mode switching (CODESYS).

## Related Projects
- Tank Level Control in FBD
- Recipe-driven Industrial Batch mixer in ST

## Author
Mohamed O. Khalifa Alsayed


