# Thermovac Sensation design notes

## Part selection

### HDMI port
Would be nice to have this, to look at a terminal or display a feed.

Consider [`DLK 54119B01121`](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/6349/54119B01121_prod.pdf), which is on Digikey as [`5622-54119B01121TR-ND`](https://www.digikey.com/en/products/detail/dlk-electronics/54119B01121/24374899).

### General purpose ADC
Just use the AD7490 again (from FOXSI power board). Nice SPI interface.

Have some general-use current sensing and voltage sensing ports, with swapable resistors.

Especially want this if we are doing active control of the bench power supply to simulate sun pointing.

## Pinout
### DB-37

NTC thermistors are pinned out on both sides, there is no common grounding in the DB-37.

The NTC thermistor numbering in the chamber starts at `7`.

| **Thermistor label** | **Topside DB pin** | **Bottom DB pin** |
|----------------------|--------------------|-------------------|
| `7`                  | `1`                | `20`              |
| `8`                  | `2`                | `21`              |
| `...`                | `...`              | `...`             |
| `24`                 | `18`               | `37`              |

Kate's notes had thermistors 14 and 15 suspicious. Number 15 seems to work fine. Number 14 reads out as OPEN. Route it anyway, we may be able to repair eventually.

## To dos:

- [x] Break out the SPI1 bus into a 0.1 inch header.
- [x] Break out some GPIOs into a 0.1 inch header.
- [x] Hook up AD7490. Break out the inputs.
- [x] Hook up the LTC2983 `nRESET` and `INTERRUPT` pins to some GPIOs.
- [x] Add logic off `nRPIBOOT` to enable USB boot _and_ powering devices on USB VBUS.
- [x] Add current limiter (900 mA) for device running off VBUS (5 V)
- [x] Route everything
- [x] Breakout CM4 headers into separate footprints for pick n place. Connector is Hirose DF40HC(3.0)-100DS-0.4V(58). Digikey has it at H124602CT-ND.
- [x] BOM for PCBWay. Compare JLCPCB.

## Impedance + fab:

### JLCPCB:

JLC04161H-3313A stackup is recommended by their stackup tool for 90 and 100 Zdiff traces.

Note this has outer copper 1 oz and inner 0.5 oz.

| **Layer** | **Mat'l** | **Thickness** |
|:---------:|:----------|:-------------:|
| Top Cu    | Cu        | 0.035 mm      |
| prepreg   | 3313 RC57 | 0.1070 mm     |
| prepreg   | 3313 RC57 | 0.0994 mm     |
| L2 Cu     | Cu        | 0.0152 mm     |
| Core      | core w/ Cu| 1.065 mm      |
| L3 Cu     | Cu        | 0.0152 mm     |
| prepreg   | 3313 RC57 | 0.0994 mm     |
| prepreg   | 3313 RC57 | 0.1070 mm     |
| Bottom Cu | Cu        | 0.035 mm      |

Using JLC's calculater, this yields:
- 90 Z traces W/S = 0.2863/0.2 mm
- 100 Z traces W/S = 0.2210/0.2 mm

for a non-coplanar differential pair.

If coupling to the coplanar GND as well, we have:
- 90 Z traces W/S = 0.2675/0.2 mm, with 0.2 mm gap to coplanar GND
- 100 Z traces W/S = 0.2042/0.2 mm, with 0.2 mm gap to coplanar GND

