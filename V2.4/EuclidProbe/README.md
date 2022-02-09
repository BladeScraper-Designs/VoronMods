# Voron2 Euclid
Info on setting up the Euclid is pretty scarce, especially for the Voron 2.4.  I put this together to hopefully help people get theirs setup.  


## STLs

For most V2 setups (and probably Trident), you'll want these STLs: 

### Probe
You'll need two M3 heatset inserts (standard Voron spec) for this.
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/OmronFotekHeatsertV3.stl

### Dock
I tried the bed mount dock, but if the gantry has not been QGL'd it struggles to deploy/stow the probe.  I found the gantry mount to be much more reliable.
You'll need an M5 nut and three M5x20 screws to install this.  It mounts on the rear left motor mount, replacing the two shorter M5 screws.  You'll have to finagle the motor wires a bit.
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/Trident_TopMountCloseV4.stl
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/Trident_TopMountDockCloseV4.stl <br>
Once it's installed and you begin testing and finding the correct dock location, you may need to adjust the dock up/down.

## Config
I started off with the default Euclid Klipper config, but I found it suboptimal and somewhat hard to understand.  I customized it, and added comments to make it easier for you to understand what you'll need to change.  I also wanted to implement auto Z, so I did.

This is my euclid.cfg.  You can either download it and add [include euclid.cfg] to your printer.cfg, or copy its contents and paste them directly into your own config.

[include auto_z.cfg]

###################################################################################################
## HOMING OVERRIDE
###################################################################################################
[force_move]
enable_force_move: True

[homing_override]   ## forces Z to raise despite not having been homed, to prevent a nozzle crash if the gantry is physically lower than something that it can hit
gcode:
   SET_KINEMATIC_POSITION Z=0
   QUERY_PROBE
   G0 Z25 F3600           ; raise bed to 15
   G28 X Y                ; home X & Y
   M402
   G0 X230 Y357
   G28 Z                  ; home Z
   G0 Z15 F3600           ; raise bed to 15

###################################################################################################
## ERROR HANDLING
###################################################################################################

[gcode_macro do_error_if_probe_deployed]
gcode:
    {% if not printer.probe.last_query %}
      {action_raise_error("probe already deployed - remove and return to dock")}
    {% endif %}

[gcode_macro error_if_probe_deployed]
gcode:
    G4 P300
    QUERY_PROBE
    do_error_if_probe_deployed

[gcode_macro do_error_if_probe_not_deployed]
gcode:
    {% if printer.probe.last_query %}
      {action_raise_error("probe unsuccessfully deployed")}
    {% endif %}

[gcode_macro error_if_probe_not_deployed]
gcode:
    G4 P300
    QUERY_PROBE
    do_error_if_probe_not_deployed

###################################################################################################
## POSITIONING
###################################################################################################

[gcode_macro move_to_dock]
gcode:
  G90
  G0 X40 Y354.5 F6000
  M400

[gcode_macro move_outside_dock]
gcode:
  G90
  G0 X100 Y354.5 F6000
  M400

###################################################################################################
## DEPLOY/STOW PROBE
###################################################################################################

# Macro to Deploy Bed Probe
[gcode_macro M401]
gcode:
    G90
    {% if printer.probe.last_query %}
      move_to_dock
      Move_outside_dock
      error_if_probe_not_deployed
    {% endif %}

# Macro to Stow Bed Leveling Probe
[gcode_macro M402]
gcode:
    G90
    {% if not printer.probe.last_query %}
     move_outside_dock           ; move toolhead to dock exit (DOCKEXIT)
     move_to_dock
     M400
     G0 Y340 F6000
     G0 X150
     error_if_probe_deployed
    {% endif %}

###################################################################################################
## REPLACEMENT MACROS
###################################################################################################

[gcode_macro BED_MESH_CALIBRATE]
rename_existing: _BED_MESH_CALIBRATE
gcode:
  QUERY_PROBE
  M401
  _BED_MESH_CALIBRATE
  M402

[gcode_macro QUAD_GANTRY_LEVEL]
rename_existing: _QUAD_GANTRY_LEVEL
gcode:
  QUERY_PROBE
  M401
  _QUAD_GANTRY_LEVEL
  M402
