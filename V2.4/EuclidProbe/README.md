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

This is my Euclid probe cfg.  You can either download it and add [include euclid.cfg] to your printer.cfg, or copy its contents and paste them directly into your own config.  I created a subdirectory in my .cfg folder called Euclid to make it a bit more organized.  If you do so, it'll be [include Euclid/euclid.cfg].  

