Here are some steps in Troubleshooting common CSI problems.

## CSI Doesn't Appear in the Dropdown in Reaper's Preferences -> Control/OSC/Web
If you're not seeing CSI in the dropdown when you go to add a surface in Reaper...

1. Make sure the .dll or .dylib is installed in your Reaper resource path's "UserPlugins" folder
2. Are you running an older Reaper build? Try updating to the latest version.
3. Mac Users: the minimum version of MacOS supported is 10.15 (Catalina). Apple Silicon is natively supported.
4. Windows Users: the Microsoft Visual C++ 2019 runtime is required. You may already have this installed on your system, but if you successfully install CSI and do not see the CSI option in Reaper's control surface preferences you will need to [download and install the 64bit version of the C++ runtime.](https://aka.ms/vs/16/release/VC_redist.x64.exe)

If none of these apply to your situation and you're still having trouble, please post in the [the main CSI thread](https://forum.cockos.com/showthread.php?t=183143) in the Reaper Control Surface's sub-forum.

## I can't find surface files for my device
First, check the CSI Support Files to check if your device is one of the included files. The CSI support files can be found [here.](https://stash.reaper.fm/v/44740/CSI%20Support%20Files.zip)

If your device is not included, does it support MCU? If yes, the MCU or X-Touch support files may work and could be considered good starting points.

If your device is not included in that list, you have two options: 1) post in the [CSI Device Setup thread](https://forum.cockos.com/showthread.php?t=245280) in the Reaper forums and ask if anyone else with the surface has files they can share or 2) read the [[Message Generators]] page and create your own .mst file for your surface. It's a one-time job. Once you've mapped all the controls on your surface, read the [[Zones]] page for information on how to map those controls. Post in the [CSI Device Setup thread](https://forum.cockos.com/showthread.php?t=245280) if you need additional help.

## My device is setup in CSI but is still not being recognized
This is a big topic with a lot of possible solutions, but the first thing to check is to make sure that CSI and your control surface are talking to each other. To do that...

1. Open Reaper
2. Run the action "CSI Toggle Show Input from Surfaces" AND...
3. Run the action "CSI Toggle Show Raw Input from Surfaces"
4. Move some knobs on your control surface and/or press some buttons.

Expected Result: 

a. You should see a Reaconsole window open up as soon as you move something on the control surface. If Reaconsole doesn't open, that points to a problem with CSI's ability to receive input from the hardware. First, **make sure your device is DISABLED in Reaper's Preferences -> Audio -> MIDI Devices**. That may sound counter-intuitive but CSI needs to be able to access your MIDI ports and if Reaper is using them, CSI cannot access them. Also, make sure some other program isn't already using the ports and they're available. 

b. If you _do_ see the Reaconsole window, you're hoping to see something like this...
```
IN <- MFTwister RotaryA1 -0.001000
IN <- MFTwister b0  00  3f 
```

...the first line is showing that the .mst works because you can see the widget name. The second line is showing the raw input from surface. If you're only seeing the second line and not the first, then that points to a problem in the .mst file or the surface itself. Things to check: does the surface have different modes? Maybe you're in the wrong one for the .mst.

Worst case, you may have to create/edit the .mst file. If all I saw was this...
```
IN <- MFTwister b0  00  3f 
```

...I'd have have to go into the .mst file and create a widget from that like this (see [[Message Generators]] for more details)...
```
Widget RotaryA1 RotaryWidgetClass
    Encoder b0 00 7f
    FB_Fader7Bit b0 00 00
WidgetEnd
```

If that all seems in order: 1) make sure your .zon files are up to date, and 2) make sure your csi.ini is up to date (you can always delete your CSI.ini and create a new one from scratch). If you're still having trouble, post in the [CSI Device Setup thread](https://forum.cockos.com/showthread.php?t=245280) for additional help. 

## Some of the new CSI features are not working
If you see something in the CSI Changelog page here but it's not working on your system, the first thing to be sure of is: does that feature exist in a "production build" of CSI or does it exist in the Exp (Experimental) builds? The Exp builds contain all of the latest features but should be considered betas. 

The Exp builds can be found [here](https://stash.reaper.fm/v/42044/CSI%20Exp.zip).

If you are running an Exp build, and a brand-new feature still isn't working as expected, please post in the main CSI thread in the Reaper Control Surface's sub-forum. You may have found a bug.

## Something I saw on this wiki isn't accurate or isn't working
Please post in [the main CSI thread](https://forum.cockos.com/showthread.php?t=183143) to report the issue. It could be as simple as the wiki not being up to date or it could be a bug. 

## Other Troubleshooting Tips
Some other troubleshooting tips...

* **Simplify:** if you have a problem where a feature in a .zon is not working as expected, try eliminating everything else, and see if the feature or .zon file works by itself. If yes, then start slowly adding things back until it stops working. That should help narrow down where the problem is. 

* **Start from scratch with a portable install.** Reaper has a "Portable Installation" option that allows you to install Reaper into another folder. You can try setting up CSI from scratch with a portable install to see if that solves your problem. If yes, the issue could be in your User Plugins folder, or with some Reaper setting.

* **Check for old .zon files or .dll's - clear them out!** Maybe you've got an old version of CSI in your user plugins folder called "reaper_csurf_integrator64.dll.bak" causing a conflict that Reaper is still picking up. Or a "buttons.zon.old" file. Clear that stuff out! When it comes to .zon files, CSI scans all .zon files within your Zone folder on launch. 

* **Maybe it's just me?** If a particular feature or something isn't working for you, but you know it's working on other people's setups, then trust that as long as you meet the minimum system requirements, it's not you. There's something amiss somewhere in your CSI setup. Check for multiple versions of zon files, installation paths, make sure your using the correct version of the .mst, proper .zon syntax, don't have old versions of things littered about. If you still need help, post in the [CSI Device Setup thread](https://forum.cockos.com/showthread.php?t=245280). It may all look right on the surface, but chances are there's some small quirk out there that's causing issues. It may just take some help and persistence to find the cause. Could be a bug too :) ! 