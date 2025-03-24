# drv-battery
Battery device tree project for setting up a simple-battery class power_supply  to use with the [bq2562x_charger driver](https://github.com/EffectiveRange/drv-bq25622)

# Usage

The device tree overlay enablement must precede the bq2562x_charger enablement, so that the BAT1 symbol is resolved properly

Good:
```bash
dtoverlay=mrhat-battery-simple
dtoverlay=mrhat-bq25622
```

Bad(won't work):
```bash
dtoverlay=mrhat-bq25622 
dtoverlay=mrhat-battery-simple
```

When using the mrhat-bq25622 overlay the battery SoC calculation internals can be emitted with the following dtparam
```bash
dtoverlay=mrhat-battery-simple
dtoverlay=mrhat-bq25622:battery_diag=true
```

This will present kernel info messages to /var/log/kern.txt (or dmesg) that can be used for calibrating the internal resistance of the battery.



# Calibrating data for a given battery type

## Measure internal resistance of a fully charged battery
- With a fully charged battery, measure the internal resistance with the method described [here](https://cellsaviors.com/blog/check-ir-multimeter), this result should be captured in `factory-internal-resistance-micro-ohms` property

## Calibrate internal resistance as a function of Vbat
- Starting with a fully charged battery attached to the bq2562x_charger based system that has `battery_diag=true`