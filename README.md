# K1 MAX Klipper Config

Klipper `printer.cfg` and tuning notes for **Creality K1 MAX**, optimized for **large-volume ABS printing**.

> 🇯🇵 日本語版は [README.ja.md](README.ja.md) を参照してください。
> ⚠️ Before using, read [DISCLAIMER.md](DISCLAIMER.md).

---

## What this is

A personal Klipper configuration for K1 MAX, tuned through actual prints — including a full-volume ABS T-Rex skull that finally came out clean after a couple of failed runs.

The config covers:

- Stepper / kinematics for K1 MAX's CoreXY geometry
- Input Shaper values measured on this specific unit
- Pressure Advance tuned for typical filaments
- Heated bed PID calibration
- START_PRINT / END_PRINT macros sized for ABS workflows
- Adjusted print speeds for stable large-format printing

This is *one* working config, not a definitive one. Mileage will vary between individual printers.

---

## Related repository

For reliable **wired Ethernet only** operation (a prerequisite for stable Klipper streaming and Moonraker), see:

➡️ [k1max-wired-network](https://github.com/tkdesign-jp/k1max-wired-network)

---

## What this is (and isn't)

- This is a **drop-in Klipper config** for K1 MAX. Use it as a starting point and tune for your unit.
- It is **not a firmware mod**. It assumes you already have Klipper running on the K1 MAX (stock or Helper-Script flavor).
- Input Shaper / Pressure Advance values are **specific to this printer** and reasonable filaments. Re-measure on yours before relying on them.
- This is **not** an official Creality config. Creality has not endorsed any part of it.

---

## ⚠️ Read this first

A wrong `printer.cfg` can:

- Cause stepper drivers to skip steps or overheat
- Drive the hotend or bed past safe limits
- Damage the build plate or hotend
- In edge cases, cause fire risk if thermal protection is misconfigured

**You apply this at your own risk.** See [DISCLAIMER.md](DISCLAIMER.md).

Before using these files:

- Back up your current `printer.cfg`
- Read every line you don't recognize
- Re-run Input Shaper measurement on your unit
- Re-run Pressure Advance calibration for your filament
- Verify thermal limits before the first print

---

## Requirements

- Creality K1 MAX (this exact model)
- Klipper firmware (stock K1 MAX Klipper or Helper-Script flavor)
- Mainsail or Fluidd accessible over the network
- SSH access to the printer

For a stable network base, also apply: [k1max-wired-network](https://github.com/tkdesign-jp/k1max-wired-network)

---

## ⚠ Hardware configuration this config assumes

This `printer.cfg` is **not** for a stock K1 MAX. It is tuned for an individually modified unit. If your K1 MAX is stock, do not load this config directly — at minimum, the temperature, PID, max_temp, and Pressure Advance values will be wrong for your hardware.

Known modifications on the reference unit:

| Component | Part | Notes |
|---|---|---|
| Hotend kit | Trianglelab **CHCB-OT** Hotend Upgrade Kit (for K1 / K1 Max / CR-M4 Sprite Extruder) | Drop-in replacement for stock K1 series hotend |
| Nozzle | [Trianglelab ZS V6](https://trianglelab.net/products/m6-zs-nozzle-hardened-steel-copper-alloy) (Hardened Steel + Copper Alloy, M6 thread) | Fits because CHCB-OT uses M6/V6 thread, **not** the stock Creality Unicorn nozzle |
| Heater | Ceramic heater coil (CHC series, ~60 W) | Faster heat-up than stock cartridge heater |
| Heatbreak | Bi-metal (titanium + copper alloy) | Part of CHCB-OT kit |
| Thermistor | PT1000 | **Not** the stock NTC100K — `sensor_type` and `pullup_resistor` in `printer.cfg` must match |

Why this matters for the config:

- **Sensor type changed**: stock is NTC100K, this build uses PT1000. Wrong `sensor_type` means temperature readings will be completely off and `verify_heater` will trigger or worse.
- **Ceramic heater (~60 W) + copper alloy nozzle** = much faster heat-up and a different thermal response. PID values are different from stock.
- **Hardened steel tip** allows abrasive filaments (carbon fibre) and higher target temperatures than the stock nozzle, but `max_temp` must still be capped to a safe value.
- **Pressure Advance** was measured with this specific nozzle / hotend combination. Re-tune for yours.
- The hotend physical dimensions (melt zone length) differ from stock, so retraction settings may need adjustment.

If your hardware differs in any of the above, **measure your own values before using this config in production**.

---

## Files in this repository

```
printer.cfg          Klipper main config
(more files to come — macros, sensor configs, etc.)
```

> 🚧 This repository is being populated. The actual `printer.cfg` will be uploaded as soon as it's sanitized (any device-specific UUIDs / paths reviewed and removed where appropriate).

---

## Installation (preview)

Once `printer.cfg` is published:

```bash
# On the printer (SSH as root)
cd /usr/data/printer_data/config/
# Back up the current config!
cp printer.cfg printer.cfg.bak.$(date +%Y%m%d)

# Pull the new config
wget https://raw.githubusercontent.com/tkdesign-jp/k1max-klipper-config/main/printer.cfg

# Restart Klipper from Mainsail UI, or:
sudo systemctl restart klipper
```

After restart:

1. Check Mainsail Console for any Klipper errors.
2. Verify hotend and bed temperatures are sane.
3. Run a short test print (e.g., a 3DBenchy at lower bed temp first).

---

## Tuning notes

Once the actual config is published, this section will document:

- How Input Shaper values were measured (frequencies for X / Y)
- Pressure Advance test print results per filament
- PID tuning values for hotend and bed
- Why certain speeds were chosen for large ABS prints
- Known issues and workarounds

---

## License

[MIT](LICENSE) — do what you like, no warranty. See also [DISCLAIMER.md](DISCLAIMER.md).
