## Minimum Version
CSI version 1.1 and greater requires Reaper version 6.16 or later. If running an older version of Reaper, CSI version 1.0 is still available on the Stash. Note: the information in the Wiki will be kept up to date for the latest CSI version.

## Download CSI
You can find the most recent [PC and Intel Mac] version of CSI here: https://stash.reaper.fm/v/42437/CSI%20v1_1.zip

An M1 compatible .dylib can be found here but please note, you will also need to download the main PC/Mac files above to get the CSI folder with the surface and zone files to get started: https://stash.reaper.fm/v/43340/reaper_csurf_integratorM1.dylib.zip

At this time, there is no Linux build but the source code is available on this github if anyone would like to attempt a port.

## CSI Installation
1. Find the Reaper resource path by running the Reaper Action: Show REAPER resource path in finder (or Windows equivalent). This is important as the folder may be different between different installs, Portable installs, etc.
2. Close Reaper.
3. Open the .zip file. The extracted folder will contain a subfolder called "CSI", which you should put in the Reaper resource path. Your CSI folder contents should look like this...
![CSI Folder Structure](https://i.imgur.com/4lyVisr.png)
4. **Windows:** Put the reaper_csurf_integrator.dll (either 32 or 64 bit according to your Reaper installation) in the folder named UserPlugins in the Reaper resource path. **Mac:** put the .dylib file in the same UserPlugins folder (requires Mac OS 10.12 or later).
5. Start Reaper. 
6. In Reaper go to Options>Preferences (or just Ctrl+P) (a new window will appear).
7. Scroll down to the bottom and click on Control/OSC/web. **Note:** while on this screen, it is recommended to uncheck the box next to "Close control surface devices when stopped and not active application" as this will disconnect CSI when Reaper is not the focused application (unless that's what you want).
8. Now, while still on the Control/OSC/Web preferences window, click on "Add" (a new window will open).
9. Click on the empty dropdown beside "control surface mode" and select Control Surface Integrator - this will now show the settings for CSI. If Control Surface Integrator is not visible, you might have to install the Microsoft Visual C++ 2019 runtime [32bit](https://aka.ms/vs/16/release/VC_redist.x86.exe) | [64bit](https://aka.ms/vs/16/release/VC_redist.x64.exe) and restart Reaper.
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

## Installing CSI Updates
If you're already running a 1.x release of CSI or later **and currently have a working configuration**, do **NOT** overwrite your current CSI.ini with the one in the .zip folder when installing updates to CSI. By the same token, do not overwrite any .zon files or zone folders you may have customized as you would be replacing your customized files and going back to stock.

Unless instructed otherwise, updates usually involve no more than copying over the .dll or dylib to the appropriate UserPlugins folder with Reaper closed, then launching Reaper. 

## EuCon Installation (Depreciated in v1.1)
**IMPORTANT NOTE:** CSI version 1.1 removed Eucon from CSI. A new Eucon project that can work side by side with CSI is underway. See the thread on the [new Eucon project](https://forum.cockos.com/showthread.php?t=255955) for additional details.

The below instructions apply only to CSI version 1.0:

1. You can find the most recent version of CSI EuCon support here: https://stash.reaper.fm/v/37947/reaper_csurf_EuCon.zip -- for Catalina, Big Sur, etc., you will need a signed version: https://stash.reaper.fm/v/41218/reaper_csurf_eucon.dylib.zip
2. With Reaper closed, install the reaper_csurf_eucon dylib/dll to the same location as the reaper_csurf_integrator.dylib/dll (e.g. UserPlugins\) 
3. Start Reaper, and setup CSI as outlined above [this is a pre-condition for Eucon to work, so do the above first]
4. Now navigate to your CSI preferences and click Add Eucon
5. Enter the number of channels you'd like to make available (e.g. if your typical project size is around 40 tracks, maybe select 48 tracks here to leave yourself some extra channels - if you added too many channels you will be wasting resources. Think of this like purchasing a console where you're custom ordering a fixed number of channels. You can always change it mid-session if necessary)
6. Enter the number of Sends and FX you would like to have available, as above if you choose too many you will be wasting resources, the default for both is 8.
7. Click OK to save the surface. Should look like this (ignore the other devices in my setup)...

![Eucon device in CSI setup](https://i.imgur.com/Zs9ZOiX.png)

8. Now close out the CSI preferences...
9. Go back to Reaper's main Preferences -> Control/OSC/Web screen
10. Click Add
11. Select Eucon from the dropdown and click ok to save. Should look like something this...

![Eucon added to Control/OSC/Web preferences](https://i.imgur.com/MM9mz4h.png)

Eucon should now be setup. Your EuCon surface should just begin working. If not, you may need to use the "Restart All Eucon Applications" in the EuControl app if the surface doesn't come immediately to life.