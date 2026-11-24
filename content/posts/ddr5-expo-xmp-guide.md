+++
title = 'Your DDR5 RAM Is Probably Running Slower Than You Paid For'
date = '2026-11-24T10:00:00+03:00'
draft = false
summary = 'A DDR5-6000 kit boots at 4800 MT/s until you enable EXPO or XMP. Here is what that costs you, how to turn it on, how to verify it, and what to do when the system will not POST afterwards.'
tags = ['Guides', 'PC Hardware', 'Memory', 'DDR5', 'EXPO', 'XMP', 'AM5']

[cover]
image = 'images/ddr5-expo-cover.jpg'
+++

This is one of the most common things I see in build lists and Discord screenshots: someone paid for a DDR5-6000 CL30 kit, and Windows reports **4800 MT/s**. Nothing is broken. The RAM is running at its JEDEC safety profile, exactly as the platform intends, until somebody tells the BIOS to load the fast profile.

If that is your machine right now, you are leaving somewhere between 5% and 20% of memory performance on the table depending on the workload — and in CPU-limited games you will feel it in the 1% lows more than in the average frame rate.

## Why DDR5 defaults to 4800 MT/s

DDR5 moved the voltage regulation onto the module itself (a small PMIC chip) and made each stick responsible for reporting its own capabilities through an SPD hub. That is a good design, but it means the platform has to be conservative on first boot: the board has no idea whether you installed a carefully binned 7200 kit or the cheapest 4800 stick on the shelf.

So the JEDEC standard profile — generally **4800 MT/s at CL40** — is what every DDR5 system boots at. It is guaranteed to work on any board, any CPU, any stick count. Everything above that is technically overclocking, even when the kit is sold as "6000 MHz" on the box.

The fast profile is stored on the module as an extra SPD table:

- **EXPO** (Extended Profiles for Overclocking) is AMD's version, introduced with AM5.
- **XMP 3.0** is Intel's, and many kits ship both.

They do the same thing: hand the board a tested combination of frequency, primary timings and voltages. Enabling it is not "overclocking" in the fiddly sense — it is loading the profile the kit was binned and validated for.

![Two DDR5 modules installed in a desktop motherboard](/images/ddr5-installed-motherboard.jpg)

## What you actually lose at 4800

Numbers make this concrete. Take a typical 2×16 GB dual-channel kit:

| | JEDEC default | EXPO profile |
| --- | --- | --- |
| Speed | 4800 MT/s | 6000 MT/s |
| Primary timings | CL40-40-40 | CL32-38-38 (example) |
| Peak bandwidth (dual channel) | 76.8 GB/s | 96 GB/s |
| First-word latency | ~16.7 ns | ~10.7 ns |

The bandwidth difference is 25%. The latency difference is about 6 nanoseconds, which sounds tiny until you remember that a modern CPU issues hundreds of millions of memory accesses per second and some of them are on the critical path.

In practice, on a Ryzen 7000/9000 system, going from 4800 CL40 to 6000 CL32 typically shows up as:

- **Small average FPS gains** in CPU-limited games, often 3–8%.
- **Larger gains in 1% and 0.1% lows**, which is what actually feels like smoothness.
- **Noticeable gains in** compression, archiving, compiling, VMs and anything else memory-bandwidth bound.
- **Almost nothing** in GPU-bound 4K gaming with a mid-range card.

It is not a night-and-day upgrade. It is free performance you already paid for.

## How to enable it

The exact menu names vary, but the path is the same on every board.

**ASUS (AM5, TUF/ROG):** press Delete at boot → **Ai Tweaker** → **Ai Overclock Tuner** → set to **EXPO I** (or XMP if your kit only carries XMP) → F10 → save and exit.

**MSI:** **OC** → **A-XMP / EXPO** → profile 1.

**Gigabyte:** **Tweaker** → **Extreme Memory Profile (X.M.P.)** → Profile1.

**ASRock:** **OC Tweaker** → **DRAM Profile Configuration** → select the profile.

Then save and reboot. First boot after enabling may take noticeably longer while the board trains the memory — that is normal, and it should not happen every time afterwards.

Some boards offer **EXPO I** and **EXPO II**. The difference is who does the tuning: EXPO I applies the module's profile as-is, EXPO II lets the board apply its own subtimings on top. If one is unstable, try the other before you start changing voltages by hand.

## Verify that it actually took

Do not trust the BIOS screen alone. Check from inside Windows:

- **Task Manager → Performance → Memory** shows "Speed" in MT/s. It should now read 6000.
- **CPU-Z → Memory tab** shows DRAM Frequency, which is **half** the effective speed: 3000 MHz means 6000 MT/s. This confuses people constantly.
- **HWiNFO64** is the better tool if you want to watch memory clocks, voltages and errors over time.

If it still says 4800, the profile did not apply. Common causes: you saved but did not confirm the "changes have been modified" prompt, the board silently fell back after a failed training attempt, or a BIOS update reset the settings.

## Then test it, because "it booted" is not stable

A memory profile can boot, run Chrome for two hours, and still throw a rounding error in the one application you care about. Test properly:

1. **TestMem5 with an anta777 config** for at least one full pass, ideally three overnight.
2. **y-cruncher** (the VST or VT3 test) for 30–60 minutes, which is brutal on the memory controller.
3. **OCCT's memory test** or **Karhu RAMTest** if you own them.
4. Then use the machine normally for a week. A single BSOD with `WHEA_UNCORRECTABLE_ERROR` or a random app crash is a data point, not a coincidence.

## If it will not POST or keeps crashing

Work through this in order — it goes from "almost certainly the fix" to "last resort":

1. **Reseat the sticks.** Genuinely. Half the failed EXPO stories end with a module not fully clicked into the slot. Listen for both latches.
2. **Use slots A2 and B2**, the second and fourth from the CPU. Two-stick kits are validated for those slots, and using A1/B1 can cost you stability at high speeds.
3. **Update the BIOS.** AGESA updates on AM5 have improved DDR5 compatibility enormously since launch. This is the single highest-value step on an older BIOS.
4. **Try the other EXPO profile** (I vs II), or drop one step, e.g. 6000 → 5600. You lose very little and gain stability.
5. **Raise SoC voltage slightly** if you know what you are doing — on AM5, 1.20–1.25 V is the usual range for 6000 MT/s, and going far above 1.30 V is not worth the risk to the memory controller. Leave VSOC on Auto first and see what the board picks.
6. **Check the QVL.** Your board vendor publishes a list of validated kits per CPU generation. A kit that is not on it can still work, but it is the first thing to suspect.
7. **Two sticks, not four.** On AM5, four DDR5 modules at 6000 MT/s is a coin flip. If you need 64 GB, buy a 2×32 kit rather than adding two sticks later.

## The 6000 CL30 sweet spot, and why faster is not always faster

On AM5, the memory clock and the Infinity Fabric clock want to run in a 1:1 ratio. At DDR5-6000 the fabric runs at 2000 MHz and everything stays synchronised. Push to 6400 or 7200 and most CPUs drop to a 2:1 mode, where the fabric runs at half the memory clock: bandwidth goes up, latency gets worse, and games often end up slower or no faster.

That is why **6000 MT/s with CL30 or CL32** has become the default recommendation for Ryzen 7000 and 9000, and why a 7200 kit is more useful on Intel than on AM5. If you already own a fast kit on AMD, try running it at 6000 with tightened timings instead of its rated speed.

## One more thing: the 40-second boot

A lot of people enable EXPO, notice the machine now sits on the motherboard logo for half a minute, and turn it back off. That is memory training, and it can be fixed.

Look for **Memory Context Restore** in the BIOS (sometimes next to **Power Down Enable**) and turn it on. The board will reuse the trained parameters instead of retraining on every boot. On some combinations it needs Power Down Enable to be stable; if you get random boot failures with it on, turn it off again and live with the slower POST.

## Bottom line

Check Task Manager. If your DDR5 kit says 4800 and the box said 6000, you are running a safety profile, not the product you bought.

Enable EXPO or XMP, confirm it in CPU-Z, test it for an hour, and then forget about it. It is the cheapest performance upgrade available to most AM5 owners, and it costs nothing but a reboot.
