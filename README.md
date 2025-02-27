# Trak_LPM
Post Processor for TRAK LPM CNC Mill with Solidworks<br>
Working on this using UPG-2

Copy the .ctl and .lng into your Solidworks Post Folder<br>
Use the .SRC and .LIB to edit and make changes.

## TODO:
1. Preloading tool change ✅
2. Figure out compensation (Currently must be disabled in Solidworks to avoid errors)

## From the Manual:

### Writing a Post Processor<br>
The following are modifications to a Fanuc 6 post-processor that are necessary for writing the ProtoTRAK post-processor.
1. Beginning file format: The ProtoTRAK has no special requirements, it does not need any special characters.
* ✅ - Already good as is.
2. End of file format: the ProtoTRAK requires the % to show the end of the file. Characters after the % will be ignored.
* ✅
3. Beginning of an operation: the ProtoTRAK requires that the tool number, feedrate and tool comp appear before, or on the same line, as a move command. The absolute zero of the ProtoTRAK is set in a different mode and does not need to be set at the beginning of each operation. The feedrate is modal, once it is set, it remains the same until changed.
* ✅ - Already good as is.
4. Coordinates: may be formatted in inch or metric. The addresses used for specifying coordinates are X, Y, Z, I, J, K. The valid ranges are:
* Inch: min -999.9999 to max +999.9999
* Mm: min -9999.99 to max +9999.999
5. Integer values MUST be followed by decimal. Ok to truncate 02.0000 into 2.0
6. Linear moves: G01 are formatted the same as rapid moves.
7. Arcs: Arc centers are specified by the address I, J and K for the X, Y and Z axes. The number following the I, J and K are incrementally referenced from the starting point of the arc. Radius values are not allowed.
8. Tool Numbers and Tool Changes: the format of the tool number is from T1 to T99. The M06 command initiates a tool change.
* ✅ - Already good as is.
9. Feed rates: the ProtoTRAK is programmed in inches (or mm) per minute using the 'F' address.
* ✅ - Already good as is.
10. Spindle speed: S1000 means 1000 RPM
* ✅ - Already good as is.
11. Turning the Spindle on: the spindle is turned on with M03 or M04 command.
* ✅ - Already good as is.
### Notes
1. Tool offsets, diameters, modifiers, home positions and work offsets are defined on the ProtoTRAK control and are ignored through G code.
2. G2 and G3 are not used for helix. See G06 and G07.
3. You must specify an I, J and K for all arcs, even if you are doing an XY plane arc where the Z axis is not moving. In this case the K value must be defined. It will be modal from there on.
4. For lead in moves where you are using G41 or G42, G41 and G42 need to be placed before the Z axis moves down to position so the tool