## Minimum Version
CSI version 2.0 requires Reaper version 6.28 or later.

## Download CSI
You can find the most recent version of CSI here: https://stash.reaper.fm/v/42437/CSI%20v1_1.zip

## CSI Installation
1. Find the Reaper resource path by running the Reaper Action: Show REAPER resource path in finder (or Windows equivalent). This is important as the folder may be different between different installs, Portable installs, etc.
2. Close Reaper.
3. Open the .zip file. The extracted folder will contain a subfolder called "CSI", which you should put in the Reaper resource path. Your CSI folder contents should look like this...
![CSI Folder Structure](https://i.imgur.com/4lyVisr.png)
4. **Windows:** Put the reaper_csurf_integrator64.dll in the folder named UserPlugins in the Reaper resource path. **Mac:** put the .dylib file in the same UserPlugins folder (requires Mac OS 10.15 Catalina or later).
5. Start Reaper. 
6. In Reaper go to Options>Preferences (or just Ctrl+P) (a new window will appear).
7. Scroll down to the bottom and click on Control/OSC/web. **Note:** while on this screen, it is recommended to uncheck the box next to "Close control surface devices when stopped and not active application" as this will disconnect CSI when Reaper is not the focused application (unless that's what you want).
8. Now, while still on the Control/OSC/Web preferences window, click on "Add" (a new window will open).
9. Click on the empty dropdown beside "control surface mode" and select Control Surface Integrator - this will now show the settings for CSI. If Control Surface Integrator is not visible, you might have to install the Microsoft Visual C++ 2019 runtime [64bit](https://aka.ms/vs/16/release/VC_redist.x64.exe) and restart Reaper.
10. There is a default Page ("HomePage") already defined to get you started. Click on the word HomePage to activate the page and see the contents.
11. Click "Add Midi", "Add OSC", or "Add EuCon". -- see more detailed instructions for EuCon below.
12. Enter the number of channels on your surface -- e.g. MCU has 8 channels. Note: if you're using a one-fader surface with a SelectedTrackNavigator, enter 0 channels here.  
13. Enter the number of Sends you would like to display -- must be no more than number of channels -- e.g. for MCU 8 Sends is maximum.
14. Enter the number of FX Menu choices you would like to display -- must be no more than number of channels -- e.g. for MCU 8 FX Menu choices is maximum.
15. Select your midi in and midi out ports (however you have your surface plugged into your computer), or your Remote IP, OSC in port, and OSC out port (however your OSC device is configured). Start any OSC device apps now.
16. Choose an mst template for your Midi surface or an OSC template for your OSC surface.
17. Choose the folder where your Zone files are located, CSI will attempt to guess based on your mst/osc choice, but you can override this. 
18. "OK" everything.
19. **While still in Options|Preferences, if your device is MIDI, go to MIDI Devices and make sure the Surfaces you wish to use with CSI are Disabled for both Input and Output.** 

## Notes
* The information entered in steps 11 through 17 are stored in CSI.ini in your CSI folder. Once you're comfortable, it's sometimes easier to edit this information directly in this file. 