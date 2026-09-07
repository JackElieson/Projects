# Active Bed Cooling for a Purple King Mattress

Research notes, first pass (Sept 2026). Goal: keep a Purple mattress comfortable
with an active (water-based) cooling pad. Hard requirements: the control unit must
be **quiet** and **compact** enough to live next to the bed. One side now, second
side (wife) optional later. Buy vs. build both on the table.

Prices below come from review sites and search snippets, not a live checkout.
Treat them as ballpark and re-check before buying.

---

## 1. Short answer

- **Air systems (BedJet) are out.** They are the loudest option (38-43 dB, and
  reviewers say cooling mode is "still pretty loud" even at 30%). Water is the
  only route that meets the quiet requirement.
- **Best buy for your constraints: Sleepme Chilipad 2.0, "Me" (single) half-king.**
  Quietest control unit on the market by every review (people check whether it is
  even running), thin stretchy pad that will not kill the Purple grid feel, no
  subscription, ~$1,600-1,800 per side. Add the second side later as a second
  identical unit.
- **Cheapest thing that actually works: ~$300-450 hybrid build.** Buy a ~$150-200
  Adamson B10 for its tubed cotton pad, throw away its weak cooler, and drive the
  pad with your own chiller and pump. This is a documented open-source pattern.
  The catch: the cheap chiller options are either not quiet, not compact, or
  need nightly ice. Details in section 4.
- **Fully sewn-from-scratch pad is not the cheap path.** The pad is the cheap
  part of any system; the chiller is where the money and noise are. Sewing your
  own only makes sense if you want a king-wide single pad or a specific layout.

---

## 2. Commercial options compared

All prices are for a **king** bed. "One side" means a half-king pad plus one
control unit. "Both sides, dual zone" means two pads and two units, each side
independently controlled.

| System | One side | Both sides (dual zone) | Unit noise | Unit placement / size | Subscription | Notes |
|---|---|---|---|---|---|---|
| **Sleepme Chilipad 2.0** | ~$1,599-1,799 (half king list $1,799; ~$200 promo running) | ~$3,200-3,600 | Quietest reviewed. "Low hum", ~4x quieter than the 1.0/Cube. No published dB. | Nightstand or floor. Dock is 6.5" tall, larger reservoir than Dock Pro. | None | Physical nightstand remote, Hydrolayer pad is thin and stretchy, 55-115 °F. Distilled water, monthly cleaner packet. |
| **Sleepme Dock Pro (Chilipad 1.0)** | ~$1,199-1,499 (being phased out, watch for clearance) | ~$1,799 (king "We") | 41-46 dB at 30 cm. Reviewers suggest a closet for light sleepers. | Bedside or under bed | None (sleep tracking free) | Same water/pad concept, older louder dock. Worth it only at a deep discount. |
| **Sleepme Chilipad Cube** | ~$594+ on sale (list higher) | ~$1,200-1,300 | Single fan speed, "pretty loud at night" | Bedside | None | Entry model. Cools ~10 °F below room. Fails the quiet requirement. |
| **Eight Sleep Pod 5 Core** | Not sold per side; whole king cover ~$3,199 | Same unit does both sides (dual zone built in) | 31-42 dB depending on load; "not silent" when working hard | Hub is a tower next to bed | **Required**: Autopilot 12 months at $199-399/yr, then $17-25/mo for features | Best software, sleep tracking, alarms. Full encasement cover, thicker top than Chilipad. Some owner reports of leaks. |
| **Adamson B10 Aqua** | ~$150-200 (twin / half-king only) | ~$300-400 (two units) | Fan on the cooler; fine on low, noticeable on high | Nightstand | None | Evaporative/fan cooler, ~5-10 °F drop, **refill water nightly**. Good pad, weak cooler. |
| **Generic Amazon "water circulating cooling pad"** | ~$100-250 | 2x | Unknown, small fans | Nightstand | None | Mostly evaporative/small TEC. Sizes are odd (63"x27"). No dependable reviews. Skip unless as a pad donor. |

### How the quiet requirement ranks them
1. Chilipad 2.0 (clear winner)
2. Eight Sleep Pod 5 (close on paper, but a subscription and a bigger hub)
3. Dock Pro (only in a closet or with a white-noise tolerance)
4. Adamson / generics (fan noise on higher settings)
5. Cube, BedJet (loud)

### Purple mattress compatibility
Purple's own guidance is that anything on top of the grid should be **thin and
stretchy** so you still sink into the grid. That rules against thick quilted
pads and foam toppers.

- Chilipad 2.0 Hydrolayer: reviewers describe it as slim, flat and stretchy,
  fits 10-16" mattresses, "virtually unnoticeable". Best match for Purple.
- Eight Sleep cover: a full zip-around encasement with a thicker top layer.
  Works on any mattress but you lose more of the grid feel.
- Adamson B10 / DIY tube pads: quilted cotton with silicone tubes inside. You
  will feel the pad more than a Chilipad. Fine for many people, but it is the
  most "layer on top" of the options.

No source specifically tested any of these on a Purple grid. Expect some
dulling of the grid feel with every option; least with Chilipad 2.0.

---

## 3. Does it need to be dual zone?

For a king with two sleepers, every water system gives you dual zone by
buying **two half-king units**, so you can start with one side and add the
second later at the same per-side price. Only Eight Sleep sells one hub that
does both sides, and it cannot be bought for one side only.

Two Chilipad docks means two boxes, one on each nightstand, each needing its
own outlet and its own distilled-water top-up.

---

## 4. DIY / hybrid options

### The physics (why the chiller is the hard part)
- A sleeping adult puts out roughly 70-100 W of heat. Perhaps 30-60 W of that
  goes into the mattress side. Budget **~60-100 W of cooling per person** once
  you include tubing losses and the pad warming from the room.
- Chilled water at 60-68 °F (15-20 °C) is what commercial pads run at. Colder
  than that feels clammy and risks condensation.
- Anything that removes ~80 W of heat from water and dumps it into the bedroom
  either has a compressor (noise, vibration, not compact) or a Peltier stack
  with fans (compact, fan noise you can engineer down, poor efficiency).

### Option A: Hybrid, buy the pad and build the chiller (the sane DIY)
Documented in the open-source ".125sleep" project (github.com/CoreyH/.125sleep).
Loop: reservoir -> pump -> pad -> chiller -> reservoir.

| Part | Approx. cost | Notes |
|---|---|---|
| Adamson B10 pad (twin / half-king), keep pad, discard cooler | $140-200 | 100% cotton, silicone tubes ~6-8 mm ID, 5-year warranty |
| 1/10 HP aquarium chiller (BAOSHISHAN, Poafamx, Hailea HC-150A class) | $150-220 | Compressor. Rated for 40-gal tanks, plenty for one pad. **Moderate noise, fridge-like, plus a fan.** Not compact (roughly a small microwave). |
| 12 V submersible pump, ~280 L/h | $12-15 | Under 35 dB, this part is quiet |
| Food-grade silicone tubing 8 mm ID, 3-4 m | $10-15 | |
| Small insulated cooler as reservoir, 5-10 L | $15-25 | Cut lid for tubing |
| Clamps, smart plug, fittings | $25-40 | Smart plug to pre-chill 30 min before bed |
| **Total, one side** | **~$315-415** | |
| **Total, both sides** (two pads, one chiller, longer runtime) | **~$455-570** | Run pads in series to keep it simple |

Performance reported: 6-10 °C (11-18 °F) below ambient, comparable to Chilipad.
Problems to expect: air bubbles (bleed the loop), algae (distilled water plus a
little cleaner), leaks at clamps.

**Verdict against your requirements:** cheapest working system, but the
compressor chiller fails "quiet and compact next to the bed". It works if you
can put the chiller in a closet or hallway and run 15-20 ft of insulated
tubing, or if you tolerate refrigerator-level noise.

### Option B: Peltier (TEC) chiller built from PC water-cooling parts
This is what the TechteamGB open-source build does: an ESP32/ESPHome board,
TEC1-12706 modules sandwiched between water blocks, a 240/360 mm PC radiator,
big slow fans, a D5/DDC pump. Cooling AND heating from the phone.

- Parts: 2-4 TEC1-12706 ($3-5 each), 2 water blocks or a TEC water-cooling kit
  ($40-60 for a 4-chip kit), 240-360 mm radiator ($40-80), 2-3 quiet 120/140 mm
  fans ($30-60), pump ($30-60), 12 V 20-30 A PSU ($30-40), controller ($10-20),
  tubing and fittings ($30). Roughly **$250-400** plus the pad.
- Pre-built "180 W thermoelectric aquarium chiller" boxes on Amazon are
  $60-120 and are the lazy version of this. They are about 6x4x6", so genuinely
  compact, but they use small loud fans and their real cooling output is well
  under the 180 W electrical rating (expect 50-80 W of cooling at useful water
  temps). Marginal for one person, not enough for two.
- Noise: this is the one architecture where quiet is under your control. Big
  radiator plus 140 mm fans at 600-800 rpm is around 25-30 dB. Peltiers
  themselves are silent.
- Downsides: Peltier efficiency is poor (COP ~0.5-1), so 80 W of cooling means
  100-160 W at the wall and 200+ W of heat into the bedroom, which partly
  defeats the purpose in summer. It is also the most engineering work.

**Verdict:** the only DIY route that can be both quiet and compact, and the
most fun, but it is a real project (electronics, thermal design, leak testing)
and the running cost and room heat are the worst of the bunch.

### Option C: Ice reservoir, no chiller at all
The "Mattress Cooler DIY kit" approach: a cooler with frozen 2 L bottles, a 12 V
pump, and the pad. ~$60-100 if you already have a cooler, essentially silent.

- Ice math: melting 1 kg of ice absorbs 334 kJ. Covering ~50 W for 8 hours needs
  ~1.44 MJ, so about **4-5 kg of ice per person per night** (two or three 2 L
  bottles). Freezer space and a nightly ritual.
- Water straight off ice is ~33 °F, far too cold for the pad; you need a
  bypass/mixing loop or a thermostat-controlled pump to hold 60-65 °F.

**Verdict:** cheapest and quietest by far, but the nightly ice routine is the
kind of thing people abandon in a month.

### Sewing your own pad from scratch
Only worth it if you want a layout no one sells (for example a single king-wide
pad, or tubing only under the torso). Pattern that works:

- Two layers of thin stretchy knit (jersey, or a cut-down Purple-style stretch
  protector) with 6-8 mm silicone tubing serpentined at 1-1.5" spacing and
  channel-stitched or tacked between them. Quilting keeps tubes from
  migrating. Keep the tube runs straight with wide loops at the ends to avoid
  kinks.
- About 15-20 m of tubing for a 30x75" torso zone, $30-50.
- Fabric, thread, elastic skirt: $30-60.
- Time: a full evening or two at the machine.

Total pad cost ~$70-110 versus $140-200 for an Adamson pad you can hack today.
The savings are small and the Adamson pad is already quilted, tested and
warrantied, so **buy the pad, build the chiller** is the better DIY split.

---

## 5. Recommendation

1. **If quiet and compact are non-negotiable and you would rather not tinker:**
   buy a **Chilipad 2.0 "Me" half-king** for your side (~$1,600-1,800; a promo
   is running through 8 Sept 2026). Add a second unit for your wife if she
   wants it after seeing yours. Sleepme sells "Certified Renewed" units; check
   that page for a cheaper second unit.
2. **If you want to spend under $500 and enjoy a build:** go hybrid (Option A)
   with the chiller relocated to a closet, or Option B if you want the challenge
   of a truly quiet bedside unit. Either way, start by buying one Adamson B10
   and living with its stock cooler for a week. If its cooling is "enough" for
   you on low fan, you are done for $200. If not, you already own the pad.
3. **Skip:** BedJet (noise), Chilipad Cube (noise), Eight Sleep (subscription,
   thicker cover, cannot buy one side).

## 6. Open questions to settle before buying
- Budget ceiling per side?
- Does your wife want her own zone now, or is one side enough to start?
- Do you care about sleep tracking / app control, or is a dial enough?
- Can the control unit live under the bed or in a closet, or must it be on the
  nightstand? (Changes whether a compressor chiller is acceptable.)
- How much tinkering appetite: weekend project, or plug-and-play?

## 7. Sources
- Sleep Foundation, best bed cooling systems 2026: https://www.sleepfoundation.org/best-sleep-products/best-bed-cooling-systems
- Mattress Nut, Chilipad 2.0 review and pricing: https://www.mattressnut.com/chilipad-sleepme-review/
- Mattress Clarity, Chilipad 2.0 review: https://www.mattressclarity.com/reviews/chilipad-review/
- WeTried.it, Chilipad 2.0 review (switched from a leaking Eight Sleep): https://wetried.it/chilipad-2-0-review/
- Boxed Sleep Reviews, Chilipad 2.0 vs Dock Pro: https://boxedsleepreviews.com/chilipad-2-0-review/
- Sleepme product pages: https://sleep.me/product/chilipad , https://sleep.me/collections/certified-renewed-products
- Sleep Foundation, Dock Pro review (41-46 dB): https://www.sleepfoundation.org/best-mattress-pads/sleepme-dock-pro-sleep-system-review
- Heal Nourish Grow, Eight Sleep Pod 5 review (pricing, subscription): https://healnourishgrow.com/eight-sleep-review/
- WeTried.it, Eight Sleep Pod 5 review (hub noise): https://wetried.it/eight-sleep-pod-5-review/
- Cybernews, Eight Sleep Pod 5 review (Core/Plus/Ultra pricing): https://cybernews.com/health-tech/eight-sleep-pod-5-review/
- Live Work Sleep, BedJet 3 review (38-43 dB): https://liveworksleep.com/bedjet-3-review/
- No Sleepless Nights, Adamson B10 review: https://www.nosleeplessnights.com/adamson-b10-review/
- Adamson B10 on Amazon: https://www.amazon.com/Adamson-B10-Aqua-Mattress-Sleepers/dp/B0C2CRPM5P
- Purple, what protector to use (thin, stretchy): https://support.purple.com/hc/en-us/articles/360028540271-What-kind-of-mattress-protector-do-I-need
- Open-source hybrid build ".125sleep": https://github.com/CoreyH/.125sleep
- Open-source conduction cooler "Bed-Cooling-is-Best": https://github.com/LTKMN/Bed-Cooling-is-Best
- TechteamGB Peltier/PC-watercooling bed cooler: https://techteamgb.co.uk/2024/11/18/i-made-a-custom-bed-watercooler-open-source-diy-bed-cooler-heater-eight-sleep-type-thing/ (video: https://www.youtube.com/watch?v=zb5H-CeDVpg)
- Mattress Cooler ice-based DIY kit: https://www.amazon.com/Mattress-Cooler-Chilled-Cooling-Systems/dp/B08ZGCVHPD
- Example 180 W Peltier aquarium chiller: https://www.amazon.com/Semiconductor-Refrigeration-Thermoelectric-Peltier-Cooling/dp/B075HBR47J
