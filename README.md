# Happy-Hare-Plus4-Configs

## ISSUES / TODO
* Macros are WIP:
    * ENABLE / DISABLE_ALL_SENSORS not working
    * RESUME and RESUME_PRINT macros needs to be reworked for HH 
    * saved variables need to be reworked
    * better handling / testing of different cases and errors
* configs in general are still WIP (definitely needs some tuning)
* no LED signalling
* probably more

## Happy Hare install

1. Copy the configs (mmu folder, box_hh.cfg and box_macros.cfg).

2. Install Happy Hare from the [WIP Plus4 repo](https://github.com/Wazzup77/Happy-Hare).

3. Run the install script `Happy-Hare/install.sh` and pray that it does not break stuff.

4. Add `mmu__revision = 0` to `saved_variables.cfg'

If you're modifying the files for development purposes you will need to rerun the install script.

## `[printer.cfg]` changes

1. Remove Qidi's stock box config include `[include box.cfg]`

2. Add `[include box_hh.cfg]` at the top.

3. Remove the `[hall_filament_width_sensor]` section

4. Make sure Happy Hare files were included during install: `[include mmu/base/*.cfg]` & `[include mmu/optional/client_macros.cfg]`

## `[gcode_macro.cfg]` changes

1. In PRINT_START we need to change Box detection logic:
```bash
[gcode_macro PRINT_START]
gcode:
[...]
-    {% if printer.save_variables.variables.box_count >= 1 %} 
+    {% if printer.mmu.num_gates >= 4 %} 
        SAVE_VARIABLE VARIABLE=load_retry_num VALUE=0 # saved variables probably need a rework
        SAVE_VARIABLE VARIABLE=retry_step VALUE=None
        SAVE_VARIABLE VARIABLE=is_tool_change VALUE=0
        {% for i in range(16) %}
            SAVE_VARIABLE VARIABLE=runout_{i} VALUE=0
            G4 P100
        {% endfor %}
        SAVE_VARIABLE VARIABLE=extrude_state VALUE=-1
-        {% if printer.save_variables.variables.enable_box == 1 %}
+        {% if printer.mmu.enabled == 1 %}
            BOX_PRINT_START EXTRUDER={extruder} HOTENDTEMP={hotendtemp}
            M400
            EXTRUSION_AND_FLUSH HOTEND={hotendtemp}
        {% endif %}
[...]
```

2. In ENABLE_ALL_SENSOR AND DISABLE_ALL_SENSOR
```
[gcode_macro ENABLE_ALL_SENSOR]
gcode:
    ENABLE_FILAMENT_WIDTH_SENSOR
    RESET_FILAMENT_WIDTH_SENSOR
    query_filament_width
    SET_FILAMENT_SENSOR SENSOR=fila ENABLE=1
-    {% if printer["filament_motion_sensor box_motion_sensor"] and printer.save_variables.variables.enable_box == 1 %}
        CLEAR_MOTION_DATA
        SET_FILAMENT_SENSOR SENSOR=box_motion_sensor ENABLE=1
    {% endif %}

[gcode_macro DISABLE_ALL_SENSOR]
gcode:
    SET_FILAMENT_SENSOR SENSOR=fila ENABLE=0
-    {% if printer["filament_motion_sensor box_motion_sensor"] %}
        SET_FILAMENT_SENSOR SENSOR=box_motion_sensor ENABLE=0
    {% endif %}
    DISABLE_FILAMENT_WIDTH_SENSOR
```

## Slicer settings

None! The mod is intended to be transparent for the slicer. If your gcode works with a stock Plus4, it should work with Happy Hare. 
Tested on Orca Slicer using the [following g-codes](https://github.com/Wazzup77/Happy-Hare-Plus4-Configs/blob/main/slicer_machine_gcodes.md), which are meant to replicate the Qidi Slicer profile.

## Additional help

Refer to the [Happy Hare documentation](https://github.com/moggieuk/Happy-Hare/wiki/Slicer-Setup).

## User interface

If you want to have a section in your printer web interface, install baseline fluidd or mainsail, which both have HH implemented.

[Happy Hare docs](https://github.com/moggieuk/Happy-Hare/wiki)