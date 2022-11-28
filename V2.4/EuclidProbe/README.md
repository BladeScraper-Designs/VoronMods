# Voron2 Euclid
Info on setting up the Euclid is pretty scarce, especially for the Voron 2.4.  I put this together to hopefully help people get theirs setup.  

# Note! It has been brought to my attention that a lot of the files on the EuclidProbe GitHub have been updated, including a new gantry mount design and new config.  I am currently in the process of testing these out and am updating the readme as I am able.

## STLs

### Dock

I am not personally a fan of the bed rail mounted dock for Euclid, so I opted to not use it.  If you decide you want to use it, you will need to make changes to the config to make it work.  Why don't I like the bed rail mount?  Well, it just doesn't work very well with a printer that has a flying, tilting gantry.  The first QGL is nearly impossible to get working in my experience, due to the fact that the printer doesn't know the toolhead's true Z position until it is QGLd, and it's hard to pick up the probe to run QGL when the printer doesn't know where the toolhead (and thus, probe mount) actually is.  Some people have gotten it working, but in my opinion for ease of build and setup, as well as deploy reliability, the gantry mount is a much better option.

For the gantry mount, you'll need these two STLs.
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/VoronGantryMountRev_4Dock.stl
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/VoronGantryMountRev_4.stl

Accompanying hardware: <br>
Attaching Dock Mount to Gantry: <br>
1 x M5x16 <br>
1 x M5x30 <br>
Attaching the dock itself to the dock mount: <br>
1 x M5x16 <br>
1 x M5 Nut <br>

You may have to fiddle with your motor wires a bit.

### Probe Mount (Toolhead) 
You've got a couple of options here.  

1. You can use the custom carriage with the mounting for the probe built in.  This is super nice, but will involve disassembling your carriage, which is kind of a pain.  Much easier on a new build using Euclid.  Note that as of now this only works with CW1.

You'll need these STLs.  Both now use M2.5 self tapping screws, instead of M3 with heatserts.

MGN9x2 (Voron 2.4 Original Design)
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/Clockwork1_2XMGN9_Native_RightM2.5.stl
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/Clockwork1_2XMGN9_Native_LeftM2.5.stl

MGN12 (Voron 2.4R2 and Trident)
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/Clockwork1_MGN12_Right_M2.5.stl
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/Clockwork1_MGN12_Left.stl

2. You can simply use the Omron adapter.  For the dock chosen above, you need this STL.  This also uses M2.5 self tappers.  There's not a lot to do here, just make sure when the probe is not attached that the mount itself is above the nozzle to avoid crashes.
https://github.com/nionio6915/Euclid_Probe/blob/main/stls/Voron/OmronFotekM2_V3.stl


## Config
# I will soon be updating this as needed using methods from the new Euclid config.  For now, though, the below instructions still work.

I started off with the default Euclid Klipper config, but I found it suboptimal and somewhat hard to understand.  I customized it, and added comments to make it easier for you to understand what you'll need to change.  I also wanted to implement auto Z, so I did.

This is my Euclid probe cfg.  You can either download it and add [include euclid.cfg] to your printer.cfg, or copy its contents and paste them directly into your own config.  I created a subdirectory in my .cfg folder called Euclid to make it a bit more organized.  If you do so, it'll be [include Euclid/euclid.cfg].  
https://github.com/BladeScraper-Designs/VoronMods/blob/main/V2.4/EuclidProbe/cfg/euclid.cfg

And this is my config for Auto Z.  If you don't want to use auto Z calibration, simply ignore this file and remove [include auto_z.cfg] from euclid.cfg. <br>
https://github.com/BladeScraper-Designs/VoronMods/blob/main/V2.4/EuclidProbe/cfg/auto_z.cfg

These configs aren't quite as easy to modify as the official Auto_Z and Euclid/Klicky configs, but they should be a decent starting point.  You may need to refer to those official configs to ensure you know what you are changing.  I tried to add notes where I can, but really can't come close to their level of detail in some places.
