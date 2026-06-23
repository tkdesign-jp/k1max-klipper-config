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
| Thermistor | NTC100K B3950 (supplied with CHCB-OT kit) | Klipper `sensor_type = "Generic 3950"`. Note: stock K1 MAX cfg lists `EPCOS 100K B57560G104F` for the same connector — slightly different beta. If migrating from a stock cfg, switch sensor_type to `Generic 3950` and re-run `PID_CALIBRATE EXTRUDER`. |

Why this matters for the config:

- **Sensor type label**: this build uses NTC100K B3950. Klipper's `Generic 3950` is the correct sensor_type. The stock K1 MAX cfg from Creality often lists `EPCOS 100K B57560G104F`, which has a slightly different beta — temperatures will drift a few degrees at high temps if mismatched.
- **Ceramic heater (~60 W) + copper alloy nozzle** = much faster heat-up and a different thermal response. PID values are different from stock.
- **Hardened steel tip** allows abrasive filaments (carbon fibre) and higher target temperatures than the stock nozzle, but `max_temp` must still be capped to a safe value.
- **Pressure Advance** was measured with this specific nozzle / hotend combination. Re-tune for yours.
- The hotend physical dimensions (melt zone length) differ from stock, so retraction settings may need adjustment.

If your hardware differs in any of the above, **measure your own values before using this config in production**.

---

## Files in this repository

```
printer.cfg          Klipper main config (with header documenting the hardware)
sensorless.cfg       Sensorless homing config (included from printer.cfg)
gcode_macro.cfg      START_PRINT / END_PRINT and helper macros
printer_params.cfg   Printer-wide parameters (kinematics, limits)
```

Notes on the bundled `printer.cfg`:

- The Creality auto-generated `SAVE_CONFIG` block (PID save data, bed mesh, prtouch defaults) has been **stripped**. Those values are per-unit calibration output and shouldn't be reused across printers. Run your own calibrations and Klipper will regenerate the block.
- The `[prtouch_v2]` section in the original file contains Creality-proprietary binary/encoded data on a few lines. Those lines are preserved exactly as the printer shipped them — **do not edit them**.
- All `[include Helper-Script/...]` files in the original cfg refer to the upstream [Helper-Script](https://github.com/Guilouz/Creality-Helper-Script) project and are not duplicated here. Install Helper-Script separately if your printer uses it.

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
