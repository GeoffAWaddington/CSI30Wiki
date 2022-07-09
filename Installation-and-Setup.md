## Minimum System Requirements
CSI version 2.0 requires Reaper version 6.28 or later.

**Windows:** 64-bit version of Windows. Additionally, the Microsoft Visual C++ 2019 runtime is required. You may already have this installed on your system, but if you successfully install CSI and do not see the CSI option in Reaper's control surface preferences you will need to [download and install the 64bit version of the C++ runtime.](https://aka.ms/vs/16/release/VC_redist.x64.exe)

**Mac:** The minimum version of MacOS supported is 10.15 (Catalina).

Linux is not supported, though CSI is open source and someone with the know-how would be welcome to attempt a Linux port. 

## Download CSI
You can find the most recent version of CSI here: https://stash.reaper.fm/v/42044/CSI%20Exp.zip [link to be updated when CSI 2.0 is official]

Surface files can be found here (the number of surfaces will be updated over time): https://stash.reaper.fm/v/44740/CSI%20Support%20Files.zip

## CSI Installation
1. Find the Reaper resource path by running the Reaper Action: Show REAPER resource path in finder (or Windows equivalent). This is important as the folder may be different between different installs, Portable installs, etc.
2. Close Reaper.
3. Open the .zip file. The extracted folder will contain a subfolder called "CSI", which you should put in the Reaper resource path. Your CSI folder contents should look like this...
![CSI Folder Structure](https://i.imgur.com/4lyVisr.png)
4. **Windows:** Put the reaper_csurf_integrator64.dll in the folder named UserPlugins in the Reaper resource path. **Mac:** put the .dylib file in the same UserPlugins folder.
5. Start Reaper. 
6. **Mac only:** you will likely receive a warning that the CSI .dylib cannot be scanned. Once Reaper launches, open Settings -> Security & Privacy and allow the CSI .dylib to run. Then restart Reaper. You may get a second message, this time, select "Open". After that, you should receive no additional warnings about CSI until you update and need to repeat this process.

## Setting up CSI for the first time
1. Once CSI is installed and you're back in Reaper, go to Options>Preferences (or just Ctrl+P) (a new window will appear).
2. Scroll down to the bottom and click on Control/OSC/web. **Note:** while on this screen, it is recommended to uncheck the box next to "Close control surface devices when stopped and not active application" as this will disconnect CSI when Reaper is not the focused application (unless that's what you want).
3. Now, while still on the Control/OSC/Web preferences window, click on "Add" (a new window will open).
4. Click on the empty dropdown beside "control surface mode" and select Control Surface Integrator - this will now show the settings for CSI. 
5. There is a default Page ("HomePage") already defined to get you started.
6. Click "Add Midi" or "Add OSC".
7. Enter the number of channels on your surface -- e.g. MCU has 8 channels. Note: if you're using a one-fader surface with a SelectedTrackNavigator, enter 0 channels here.  
8. Enter the number of Sends you would like to display -- must be no more than number of channels -- e.g. for MCU 8 Sends is maximum.
9. Enter the number of FX Menu choices you would like to display -- must be no more than number of channels -- e.g. for MCU 8 FX Menu choices is maximum.
10. Select your midi in and midi out ports (however you have your surface plugged into your computer), or your Remote IP, OSC in port, and OSC out port (however your OSC device is configured). Start any OSC device apps now.
11. Choose an mst template for your Midi surface or an OSC template for your OSC surface.
12. Choose the folder where your Zone files are located, CSI will attempt to guess based on your mst/osc choice, but you can override this. 
13. "OK" everything.
14. **While still in Options|Preferences, if your device is MIDI, go to MIDI Devices and make sure the Surfaces you wish to use with CSI are Disabled for both Input and Output.** 

## Installing CSI Updates
When a new version of CSI is released, updating CSI involves no more than downloading the latest version and copying over the .dll or dylib to the appropriate UserPlugins folder with Reaper closed, then launching Reaper. Major versions may introduce new syntax or change existing syntax. When this occurs, this Wiki will be updated to reflect the latest additions or changes.

If you plan on updating any surface or support files, do **NOT** overwrite your current CSI.ini with the one in the .zip folder. By the same token, do not overwrite any .zon files or zone folders you may have customized unless you are intentionally going back to a stock setup - you will lose any customizations you made.

## Notes
* The information you entered while setting up CSI for the first time gets stored in [[CSI.ini|CSI.INI]] in your CSI folder. Once you're comfortable, it's sometimes easier to edit this information directly in this file. 