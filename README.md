Printer: Anet A8
Board version: Anet 1.7
Z Probe: L12A3-4-Z/BX
Filament sensor: None
Firmware: Marlin 2.0.7.2

Main features enabled:
SDSupport
Z Probe auto bed level
BAUD Rate 250000 to optimize Octoprint RPI 3B+

Disabled features:
Boot screen
PRINTJOB TIMER AUTOSTART
STRING CONFIG H AUTHOR
SDcard print confirmation

Actual binary size: 126616 bytes (99%)


To setup Z Probe Offset

M851 Z0 ; set Z offset to 0

M500 ; Save to EEPROM

M503 ; report EEPROM, double check Z offset is zero

G28 ; Home all axes

M104 SXXX ; Set extruder temp, replace XXX with your current materials extrusion temp

M140 SXX ; Set bed temp, again replace XX with your current materials heat bed temp

after target temps are met:

G0 Z0 ; send nozzle to current zero position above bed, observe this position

M211 S0 ; Disable endstops, proceed with caution

place a sheet of paper on the bed

lower your Z axis 0.1 mm at a time until your nozzle lightly grips the paper

M114 ; Get current position, note first Z position reported here, should be negative, mine is Z-0.7

M851 Z-X.X ; Set Z offset, replace X.X with your results from M114

M500 ; Save to EEPROM

M503 ; Report EEPROM, double check M851 output

M211 S1 ; Turn endstops back on

M500 ; Save

M119 ; Report endstop states, double check they're back on, then proceed

G28 ; Home all axes

G29 ; Probe bed

M500 ; Save results to EEPROM

## Changing fan duct (PID Auto Tune)

This step is very important, so don't blame the fan duct if you have not performed it and you are getting bad results :)
Every time you install a new fan duct, it's highly recommended to perform the so-called "PID Auto Tune" procedure on your heat block. It's performed very easily - when your printer is cold, connect it to your PC, and start your preferred program that allows you to send GCODE commands directly to your printer. You can use Pronterface (http://www.pronterface.com/) or whatever you prefer (some slicers have this function built in).
Next, decide what's your preferred print temperature - in my case it's 190C.
You will also have to turn the fan ON.
Execute the following GCODE command to turn it on:

M106 S255 
Now run the PID auto tune GCODE command (replace "190" with whatever temperature you use):

M303 E0 S190 C8
It will respond with
Info:PID Autotune start

Your printer will go through 8 cycles of heating / cooling, so it will take a couple of minutes.
In the end, you will get a response, that looks something like this:

bias: 92 d: 92 min: 196.56 max: 203.75
Ku: 32.59 Tu: 54.92
Clasic PID
Kp: 19.56
Ki: 0.71
Kd: 134.26
PID Autotune finished ! Place the Kp, Ki and Kd constants in the configuration.h

Copy this response somewhere, otherwise it will quickly get lost in the idle "wait" messages.
Now go to save your settings with the command (replace values):

M301 P[Kp] I[Ki] D[Kd] 

And save with:

M500

When you have tuned your heater properly, the temperature will fluctuate by just plus/minus 0.5 - 1C and will be very stable.