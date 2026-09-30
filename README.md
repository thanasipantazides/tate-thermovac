# Tate 220 ThermoVac Chamber

Here lies a PCB design and code for instrumentation of the UMN Tate Hall rm 220 thermal-vacuum chamber.

The PCB design is in KiCad 10 format. There may be some custom footprint and symbol libraries missing from this distribution; I need to figure out how to bundle all this into the repository.

This board is supposed to read out ~17 NTC thermistors in the chamber, digitize them, and do something useful with the values. It also exposes some general-purpose ADC channels, an SPI, and a UART for peripheral communication. In addition to USB and Ethernet.

