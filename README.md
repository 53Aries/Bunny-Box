# Happy-Hare-Plus4-Configs

## ISSUES
* Macros not fully tested 
* configs in general are in a very very experimental, constantly shifting state
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

2. Add '[include box_hh.cfg]` and `[include box_macros.cfg]]`
Make sure Happy Hare files were included during install: `[include mmu/base/*.cfg]` & `[include mmu/optional/client_macros.cfg]`

3. Remove the `[hall_filament_width_sensor]` section

## Slicer settings

Refer to the [Happy Hare documentation](https://github.com/moggieuk/Happy-Hare/wiki/Slicer-Setup).

## User interface

If you want to have a section in your printer web interface, install baseline fluidd or mainsail, which both have HH implemented.

[Happy Hare docs](https://github.com/moggieuk/Happy-Hare/wiki)