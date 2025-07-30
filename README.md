# Teardown of a smart switched outlet I bought

This is a KiCAD 9.X project. KiCAD is a free electronic design tool [available at kicad.org](https://kicad.org/).

## Product overview
The switched outlet is sold by Meross.
They describe it as a Matter smart plug, but it is a switched outlet on a small box which plugs into your wall outlet.
It uses WiFi over IPv6 over WiFi. It does not use Thread.
This is for US voltage and outlets. It was purchased in July 2025.
The device allows switching of the outlet and measurement of the power and energy used by the connected device.

## Design outline
The product contains a transformerless switching power supply to generate a neutral-referenced
5V supply. It has a 3.3V low dropout linear regulator to further reduce this.
It contains a sub-module which has a RealTek Bluetooth radio
and controller which uses the 3.3V supply.
It measures current as a voltage differential across a 1mΩ sense resistor on the neutral side.
It has a resistor chain from the hot side to determine the line voltage so power can be calculated.
It uses a 277V/16A-rated relay on the hot side to turn the connected device on and off.
The board has about 3mm of creepage clearance to components on the hot (switched) side.
It has less clearance between the unswitched hot and switched not, only 1mm. But this gap
is improved by using a 1mm slot.
The protective ground pin is not connected to anything inside except for the output receptacle.
The controller uses a GPO to turn the relay on and off through an NPN transistor.
The controller receives the voltage, current and calculated power data over a low voltage
RS-232 interface at 4800bps 8N1.
The power input is fused with a self-resetting polyfuse and uses a metal-oxide varistor to limit
inrush current.
The microcontroller uses a negative temperature coefficient thermistor to measure the temperature
of the device, presumably for protection.

## Design quirks

Each of these are small deviations from expected. Not every one may matter.

The used power supply chip suggests using a full-bridge rectifier for its AC input.
However this design uses half-wave. It does however put two diodes in series so that
a single diode failing short does not produce a short. This is probably a reasonable design
choice.

The device contains no X-class capacitor at all. One is suggested for the power supply chip
but it is omitted. With no non-resetting fuse inside this may be the right choice. It does
seem to go against the company claims of "protection against short-circuits" somewhat.
Even if this has some software in the microcontroller to turn off on a measured overcurrent
or over-temperature this device is relying on your wall circuit's protection
for true overcurrent protection.

The wireless interface chip is rated by the manufacturer at 1000mA current draw. It is
powered by a 3.3V LDO which is in a package too small to be rated at 500mA. It surely
is rated at 300mA or less. Despite this the LDO has 20µF of capacitance on the output
side. This normally would be for a 2A regulator. In this case it seems that these
capacitors are there to try to make enough instantaneous current available to overcome
the underrating of the LDO. This seems to work fine but no measurements were made.

The small slit around the single button is not gasketed unlike another smart switched
outlet I purchased. This would mean liquids could ingress. It is nearest a low voltage
portion of the board and of course the button. The device of course also has the
receptacle on the front and those are not sealed so water could ingress there also
as it could on any similar device.

There are small quirks about the board layout. Unnecessary vias, etc. None of these
are of importance.

## Device identification
The device is approximately 70mm wide by 40mm high by 40mm deep.
It has a NEMA 5-15P on the back to plug into a US outlet. It has
a NEMA 5-15R on the front for plugging in the device you wish to switch.
It is rated for 15 amps in and out and "120V" (presumably RMS nominal).

On the back it is identified as:

Smart WiFi Plug Mini

Model: MSS315

It lists as containing FCC ID 2AMUU-MWA05 and IC 24963-MWA06

