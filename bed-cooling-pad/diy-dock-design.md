# DIY Dock for a Chilipad 2.0 Hydrolayer

Design outline for a quiet, nightstand-sized control unit that drives one or
two Sleepme Hydrolayer pads. Heating and cooling. Draft v1, Sept 2026.

## 1. The shape of the thing

Think "small form-factor PC case with a 240 mm radiator", not ATX. Everything
below fits in a mini-ITX cube that has a 240 mm radiator mount (roughly
10-12 liters, 260 x 280 x 300 mm), or a 3D-printed / plywood shell built
around the radiator. Front: small OLED and a rotary knob. Back: two CPC
bulkhead fittings per zone, IEC power inlet. One face: the radiator, fans
blowing out.

For two zones, the same case with a 360 mm radiator on one face, or two
240s on two faces.

## 2. Two water loops, thermally joined by the Peltier sandwich

```
COLD LOOP (one per zone, touches the pad)

  [reservoir 0.5-1 L] --> [12 V pump A] --> [cold-side water block]
        ^                                          |
        |                                          v
  [CPC return] <---- Hydrolayer pad <---- [CPC supply]
                                (thermistor T_supply here)

PELTIER SANDWICH

  cold-side block | TEC | TEC | hot-side block      (2x TEC1-12706 per zone)
                       ^^^^ polarity reversed = heating

HOT LOOP (shared by all zones, never touches the pad)

  [D5/DDC pump-res combo] --> [hot-side block(s)] --> [240/360 mm radiator + PWM fans]
        ^                                                     |
        +-----------------------------------------------------+
                                (thermistor T_hot here)
```

Why two loops: the pad water must stay clean, low-pressure and sealed. The
hot side wants a high-flow PC pump and a big radiator. Joining them through
the Peltier plates keeps the pad loop simple and lets the hot loop be loud
or quiet on its own terms (it will be quiet: big radiator, slow fans).

Why water-to-water rather than heatsinks on the hot side: a 240 mm radiator
sheds 300 W at 700-900 rpm with fans you cannot hear from the pillow. Two
tower heatsinks doing the same job are bulkier and louder.

## 3. Sizing

| Item | One zone | Two zones |
|---|---|---|
| TECs (TEC1-12706, 12 V, ~6 A each) | 2 | 4 |
| Cooling delivered at 60-65 °F water | 60-100 W | 120-200 W |
| Heat to reject on hot side | ~250 W | ~500 W |
| Radiator | 240 mm | 360 mm (or 2 x 240) |
| 12 V PSU | 20 A (240 W) | 40 A (480 W), or run TECs at 9 V |
| Cold-loop pumps | 1 | 2 |
| Hot-loop pump | 1 D5 or DDC | 1 D5 |

Run the TECs at 8-10 V rather than 12 V. Their efficiency (COP) roughly
doubles at partial voltage and you lose little capacity. PWM from the
controller does this for free.

## 4. Electronics

### Controller: ESP32 running ESPHome
No firmware to write. A YAML config declares sensors, outputs, and a climate
controller. It exposes the dock to Home Assistant for phone control and
schedules, and works standalone via the knob if Wi-Fi is down.
TechteamGB's open-source build is the reference config to copy from.

### Per zone
- **TEC driver: BTS7960 H-bridge module (43 A).** One module per zone gives
  PWM power control AND polarity reversal from two GPIO pins. Polarity
  reversal is what turns cooling into heating. No relay needed.
- **Pump A:** 12 V brushless, 3-5 W, ~3 L/min, switched by a logic-level
  MOSFET (IRLZ44N or a 15 A MOSFET module).
- **T_supply:** DS18B20 waterproof probe in a tee on the pad supply line.
  This is the number the thermostat controls to.
- **T_return** (optional, nice for data): second DS18B20 on the return.
- **Float switch** in the reservoir (low water -> TEC off, alert).

### Shared
- **Hot pump:** D5 or DDC, PWM or just on at fixed speed.
- **Fans:** 2-3 x 120/140 mm 4-pin PWM, 25 kHz PWM from the ESP32, tach read.
- **T_hot:** DS18B20 on the hot loop. Safety limit.
- **Leak sensor:** two bare probes on the case floor (or a rope sensor).
  Wet -> everything off, buzzer.
- **Display + input:** 128x64 OLED (I2C) + rotary encoder with push.
  Turn = setpoint, push = mode (off / cool / heat / schedule).
- **Buzzer** for faults.
- **Power:** 12 V PSU -> BTS7960s, pumps, fans. 12 V -> 5 V buck for ESP32.

GPIO budget for two zones: ~14 pins. An ESP32 DevKit has plenty.

## 5. Control logic

1. **Startup:** hot pump on, fans to 40 %, cold pump(s) on. Wait 10 s.
   Confirm fan tach and no leak. Only then enable TECs.
2. **Thermostat:** ESPHome `climate` (PID or bang-bang with 1 °F hysteresis)
   on T_supply. Cool mode target 58-68 °F; heat mode 85-105 °F.
3. **TEC power:** PID output -> BTS7960 PWM duty. Cap at ~75 % (about 9 V)
   for efficiency and TEC life.
4. **Fan curve:** by T_hot. 30 % below 35 °C, ramp to 100 % at 50 °C.
5. **Interlocks (any one -> TECs off, alert):** T_hot > 55 °C, fan tach 0,
   float low, leak wet, T_supply sensor missing, T_supply < 50 °F (over-cool).
6. **Schedules:** in Home Assistant, or ESPHome `time` triggers: pre-cool
   30 min before bed, warm up 20 min before the alarm.

## 6. Plumbing details

- **Pad connection:** CPC DPC-series bulkhead couplings on the back panel.
  Sleepme's hose plugs straight in. Buy the mating halves from McMaster-Carr
  or US Plastic (search "CPC DPC" or "CPC PLC"; take the pad hose to match).
- **Cold loop tubing:** 8 mm ID silicone or 10 mm ID PVC, short runs, hose
  clamps on every barb.
- **Hot loop:** standard PC 10/13 mm soft tubing and G1/4 compression fittings.
- **Reservoir:** small PC tube reservoir or a bottle with a fill cap, mounted
  at the top of the case so it is the high point (easy fill, easy bleed).
  Keep it below mattress height so a leak cannot siphon the pad.
- **Water:** distilled plus a few drops of PC biocide (or a monthly capful of
  3 % hydrogen peroxide) in the cold loop. Standard PC coolant in the hot loop.
- **Pressure:** the Hydrolayer wants gentle flow. Pump A should be a low-head
  (<2 m) pump. Do not use a D5 on the cold loop.

## 7. Parts list, one zone (two-zone deltas in brackets)

| Part | Approx. |
|---|---|
| 2x TEC1-12706 [4x] | $10-20 |
| Cold + hot water blocks sized for two TECs (or a "TEC water cooling kit" that includes blocks, TECs and small pumps) | $40-70 [$80-120] |
| 240 mm radiator [360] | $40-70 |
| 2x 120 mm PWM fans, quiet [3x] | $30-60 |
| D5/DDC pump-reservoir combo | $50-80 |
| 12 V cold-loop pump [2x] | $15-25 |
| 12 V 20 A PSU [40 A] | $25-35 [$45-60] |
| ESP32 DevKit | $8-12 |
| BTS7960 module [2x] | $8-12 each |
| DS18B20 probes x3 [x5], float switch, leak probes, buzzer | $20-30 |
| OLED + rotary encoder | $10-15 |
| MOSFET module, 5 V buck, wire, connectors, fuse holder | $20-30 |
| CPC bulkhead couplings, fittings, tubing, clamps | $40-60 |
| Mini-ITX cube case with 240 mm mount, or printed/plywood shell | $50-90 |
| **Total** | **$370-620** [$480-780] |

Higher than the earlier rough estimate because this includes the case, the
display, the H-bridge and the sensors that make it safe and usable rather
than a bench experiment. Still well under the $950 Sleepme dock.

## 8. Build order

1. Bench: PSU + ESP32 + BTS7960 + one TEC on a heatsink. Prove PWM and
   polarity reversal.
2. Assemble the Peltier sandwich and both loops on the bench, with a bucket
   as the "pad". Bleed. Run 2 hours. Log T_supply and T_hot.
3. Tune fan curve for noise. Target: inaudible from 1 m at steady state.
4. Mount everything in the case. Leak test 24 h with paper towels under every
   fitting.
5. Connect the Hydrolayer in the bathtub (dry tub, pad laid flat). Run
   overnight.
6. Move to the bed. Tune setpoint over a week.

## 9. Things that will bite you if skipped
- TECs powered before the hot loop is moving (they cook in seconds).
- No thermal paste or uneven clamping on the sandwich (halves your cooling).
- A cold-loop pump with too much head (stresses the membrane).
- Reservoir above mattress height (siphon on a leak).
- No fuse between PSU and BTS7960.
- Fans at 100 % because the radiator was too small. Buy the 240.
