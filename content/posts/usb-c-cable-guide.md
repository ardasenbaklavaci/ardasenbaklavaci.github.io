+++
title = 'Not Every USB-C Cable Is the Same: How to Tell What You Actually Bought'
date = '2026-11-24T09:00:00+03:00'
draft = false
summary = 'USB-C is a connector shape, not a standard. The same-looking cable can be 480 Mbps or 40 Gbps, 15 W or 240 W. Here is how to read the specs, test what you have, and stop buying the wrong one.'
tags = ['Guides', 'Consumer Tech', 'USB-C', 'Cables', 'Buying Guide']

[cover]
image = 'images/usbc-cables-cover.jpg'
+++

Every USB-C cable in your drawer looks identical. They are not. One of them will charge a laptop at 100 W and move a 4K display signal at 60 Hz. Another one, physically indistinguishable, is a 480 Mbps charging cable that will do neither, no matter what adapter you plug into it.

This is the single most confusing part of modern consumer tech, and it exists because **USB-C is a connector specification, not a capability specification**. The oval plug says nothing about what is inside the cable, and most manufacturers do not print it on the jacket either.

## What a cable can differ in

There are four independent things a USB-C cable may or may not support:

| Capability | Range across "USB-C cables" |
| --- | --- |
| Data speed | 480 Mbps, 5 Gbps, 10 Gbps, 20 Gbps, 40 Gbps (80/120 on USB4 v2 and Thunderbolt 5) |
| Charging | 15 W default, up to 100 W (20 V × 5 A), up to 240 W with USB PD 3.1 EPR |
| Video | None, or DisplayPort Alt Mode / Thunderbolt tunnelling |
| Other | USB4, Thunderbolt 3/4/5, Ethernet, debug, e-marker presence |

A cable that supports 240 W may still be limited to USB 2.0 data. A cable that runs 40 Gbps may be rated for only 60 W. There is no rule that ties them together, and cheap bundles exploit exactly that.

## The 480 Mbps trap

The most common cable in the world is the one that comes in the box with a cheap accessory: **USB 2.0 data at 480 Mbps, 60 W charging**. It is fine for a mouse, a keyboard, or charging a phone overnight. It is useless for almost everything else.

480 Mbps is roughly **60 MB/s** in real terms. Copying a 50 GB game backup over one takes about fifteen minutes longer than it should, and it cannot drive a display at all, because USB 2.0-only cables physically lack the high-speed pairs that DisplayPort Alt Mode uses.

If you have ever plugged a monitor into a USB-C port with a random cable and got "no signal", this is why. It is not the port, the laptop or the monitor.

![Macro shot of a USB-C connector](/images/usbc-connector-macro.jpg)

## The e-marker is the thing to know about

Any cable rated above **3 A** (so, 60 W and up) must contain a small chip called an **e-marker** that tells the charger what the cable can handle. Cables rated for high-speed data also use it to advertise their speed.

This matters because the charger and the device negotiate power over the cable, and they rely on the e-marker to avoid melting a thin wire. No e-marker, no 5 A, no 100 W — the negotiation caps at 60 W at best, and often 15 W.

USB PD 3.1's **240 W (48 V at 5 A)** requires an EPR-capable e-marker. A 100 W cable plugged into a 240 W charger will simply negotiate 100 W. Nothing breaks, you just do not get the speed you expected.

## Practical tests you can run in two minutes

You do not need a lab. You need three data points.

1. **Charging test.** Plug the cable into your most powerful charger and your laptop, and read the wattage in the laptop's battery settings or with a USB-C power meter (they cost less than a game). If a 100 W charger delivers 45 W, the cable is the suspect.
2. **Data test.** Copy a large file (10 GB or more) between two fast SSDs over the cable and watch the transfer rate in Task Manager or Finder. Around 40 MB/s means USB 2.0. Around 400 MB/s means 5 Gbps. Around 900 MB/s means 10 Gbps.
3. **Display test.** If you have a USB-C monitor, plug in and see. If nothing happens, the cable has no high-speed pairs.

If you want to know for certain, a **USB-C cable tester** (the small OLED meters from brands like AVHzY or similar) reads the e-marker directly and reports speed, current rating and vendor. For anyone with a drawer full of anonymous cables, it pays for itself in avoided returns.

## What the labels actually mean

USB naming has been a mess for years, so here is the current state in plain terms:

| Marketing name | Data speed | Typical use |
| --- | --- | --- |
| USB 2.0 | 480 Mbps | Charging, mice, keyboards |
| USB 3.2 Gen 1 (formerly USB 3.0 / 3.1 Gen 1) | 5 Gbps | External SSDs, hubs |
| USB 3.2 Gen 2 | 10 Gbps | Fast external SSDs |
| USB 3.2 Gen 2x2 | 20 Gbps | Rare, mostly on desktop ports |
| USB4 20 / 40 Gbps | 20 or 40 Gbps | Docks, eGPUs, displays, fast storage |
| Thunderbolt 3 / 4 | 40 Gbps | Docks, displays, daisy chains |
| Thunderbolt 5 | 80 Gbps (120 in a boosted mode) | High-end docks and displays |

Note that "USB 3.2" alone tells you nothing: Gen 1 and Gen 2 differ by a factor of two. If a listing does not state the generation, assume the slowest option.

## Length limits are real

Passive cables have physics working against them:

- **40 Gbps**: usually 0.8 m or shorter for passive copper.
- **10 Gbps**: around 2 m is the practical limit.
- **5 Gbps**: around 2–3 m.
- **USB 2.0**: can be several metres, because the signalling is far slower.

A 3 m "USB4 40 Gbps" cable at a suspiciously low price is either active (has a redriver chip, which is legitimate and more expensive) or not what it claims.

## What to actually buy

If you want one cable that handles everything, buy a **USB4 / Thunderbolt 4 cable rated 40 Gbps and 240 W (or 100 W)** from a brand that publishes the spec sheet. One good cable costs less than three bad ones and removes all the guessing.

For the rest:

- **Charging-only cables** are fine for phones, controllers and accessories. Label them with a bit of tape so you stop grabbing them for data.
- **Do not buy "USB-C to USB-C 100W" listings with no data rating** if you plan to connect a drive or a monitor. They are charge cables with a marketing title.
- **Check the port, not just the cable.** A 40 Gbps cable in a 5 Gbps port gives you 5 Gbps. Laptop manufacturers are not consistent about which of their identical-looking ports is which; the spec sheet is the only reliable source.
- **Avoid the gas-station cable.** There are cables on the market with no e-marker, thin conductors and no shielding, sold at prices that make sense only if you assume copper is free.

## The slow-charging checklist

If your phone or laptop charges slower than expected, check these in order:

1. **The cable** — especially if it is the one that came in a cheap accessory box.
2. **The charger** — a 20 W phone charger will not fast-charge a laptop, and PD negotiation always picks the lowest common denominator.
3. **The port** — some laptops only deliver full power on one specific USB-C port.
4. **The device's charging curve** — most phones deliberately slow down above 80%, so a drop in wattage near full is normal, not a fault.
5. **Lint in the port.** Genuinely the most common cause of "it only charges if I hold it at an angle". A wooden toothpick, no metal, and a light touch.

## Bottom line

The plug is not the standard. The cable is a bundle of copper, shielding and a tiny chip, and the price difference between a good one and a bad one is a few dollars.

Buy one certified 40 Gbps / 240 W cable for the things that matter, keep the slow ones for charging, and label them. Your future self, hunting for a cable that will actually drive a monitor, will thank you.
