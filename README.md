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