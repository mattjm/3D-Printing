# SuperSlicer to OrcaSlicer profile conversion errata

Or "why is my flow calibration perfect during the test but my prints have giant gaps in the walls?"

[Ellis' superslicer PIF profiles](https://github.com/AndrewEllis93/Ellis-SuperSlicer-Profiles) are well known.  There is a tool to convert SuperSlicer to OrcaSlicer profiles:

https://github.com/theophile/SuperSlicer_to_Orca_scripts

HOWEVER if you blindly convert Ellis's profiles to OrcaSlicer, "you're gonna have a bad time".  

The conversion script converts the SuperSlicer `extrusion_multiplier` to OrcaSlicer's `print_flow_ratio`.  The value in Ellis' PIF profile for `extrusion_multiplier` is 0.924 (for the 15mm^3 standard PIF profile).

OrcaSlicer `print_flow_ratio` is _not typically visible in the UI_!  But it is applied globally and you can see it if you switch to per-object settings, where it shows up on the **Quality** tab under **Walls and Surfaces** as **Flow Ratio**.  

![screenshot of orcaslicer settings](https://github.com/mattjm/3D-Printing/blob/main/images/Screenshot%202026-06-28%20at%2008.32.58.png?raw=true)

The built-in calibration tests adjust the flow for each test object using this exact value.  Meaning it's overwritten for calibrations, so it's not a multiplier that cancels out because it's part of the calibration.  

If you blindly convert Ellis' profiles, that `print_flow_ratio` is a factor in your final flow calculation so you _will_ get underextrusion in walls.  If you had a filament flow of 0.95 then your actual flow would be .878.  

My solution was to manually adjust `print_flow_ratio` in the OrcaSlicer JSON printer config to 1.0, and set my extrusion multiplier to the correct calibrated value in the OrcaSlicer filament profile (make sure OrcaSlicer is closed when you do this).  This solved the mysterious wall gaps I was having when I printed at what was supposed to be the correct flow.  

As of 2026-06-28, The OrcaSlicer printer profiles on Mac are in:  /Users/<username>/Library/Application Support/OrcaSlicer/user/default/process

It might be easier to adjust `extrusion_multiplier` in the original profile before converting it.  
