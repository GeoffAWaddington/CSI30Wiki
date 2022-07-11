## Minimum System Requirements
CSI version 2.0 requires Reaper version 6.28 or later.

**Windows:** 64-bit version of Windows. Additionally, the Microsoft Visual C++ 2019 runtime is required. You may already have this installed on your system, but if you successfully install CSI and do not see the CSI option in Reaper's control surface preferences you will need to [download and install the 64bit version of the C++ runtime.](https://aka.ms/vs/16/release/VC_redist.x64.exe)

**Mac:** The minimum version of MacOS supported is 10.15 (Catalina). Apple Silicon is natively supported.

Linux is not supported, though CSI is open source and someone with the know-how would be welcome to attempt a Linux port. 

## Download CSI
You can find the most recent version of CSI here: https://stash.reaper.fm/v/44868/CSI%20v2_0.zip

Surface files can be found here (the number of surfaces will be updated over time): https://stash.reaper.fm/v/44740/CSI%20Support%20Files.zip

## CSI Installation
1. Find the Reaper resource path by running the Reaper Action: Show REAPER resource path in finder (or Windows equivalent). This is important as the folder may be different between different installs, Portable installs, etc.
2. Close Reaper.
3. Open the CSI zip file you downloaded.
4. **Windows:** Put the reaper_csurf_integrator64.dll in the folder named UserPlugins in the Reaper resource path. **Mac:** put the .dylib file in the same UserPlugins folder.
5. If a first time setup, [[install the CSI support files|Installation-and-Setup#Installing-CSI-Support-Files]] then start Reaper. 
6. **Mac only:** you will likely receive a warning that the CSI .dylib cannot be scanned. Once Reaper launches, open Settings -> Security & Privacy and allow the CSI .dylib to run. Then restart Reaper. You may get a second message, this time, select "Open". After that, you should receive no additional warnings about CSI until you update and need to repeat this process.

## Installing CSI Support Files
The first time you install CSI, you will need to additionally add a CSI folder to your Reaper Resource path, along with Surface and Zone subfolders. Surface and Zone folders will be provided for some common control surfaces to help get you up and running quickly.

1. In File Explorer (Windows) or Finder (Mac) open your Reaper Resource Path
2. Open the CSI Support zip file you [[downloaded| https://stash.reaper.fm/v/44740/CSI%20Support%20Files.zip]]
3. Copy over the CSI folder from the .zip file directly into your Reaper Resource path. The file structure should look like the one shown in the image below. 
![CSI Folder Structure](https://i.imgur.com/4lyVisr.png)

## Setting up your CSI devices for the first time
Now that you have successfully installed CSI and the Support Files, the next step is to setup your devices for the first time.

1. In Reaper, go to Options>Preferences (or just Ctrl+P) (a new window will appear).
2. Scroll down to the bottom and click on Control/OSC/web. **Note:** while on this screen, it is recommended to uncheck the box next to "Close control surface devices when stopped and not active application" as this will disconnect CSI when Reaper is not the focused application (unless that's what you want).
3. Now, while still on the Control/OSC/Web preferences window, click on "Add" (a new window will open).
4. Click on the empty dropdown beside "control surface mode" and select "Control Surface Integrator" - this will now show the settings for CSI. 
5. There is a default Page ("HomePage") already defined to get you started. If you do not see one, click "Add" under the Pages section and create a page called "HomePage".
6. Under the "Surfaces" section on the left, click "Add MIDI" or "Add OSC" depending on what type of Surface you are trying to add.
7. **If adding a MIDI surface**, enter a name for your surface, and select the MIDI In and Out ports that correspond to your device. **Important note:** the MIDI Devices must be disabled in Reaper's Preferences -> MIDI Devices in order for CSI to access the MIDI ports.
8. **If adding an OSC surface**, enter a name for your surface. In the "Remote Device IP Address" field, add the IP address of the device (phone, tablet, PC) running the OSC host software. Next, enter the "CSI receives on port" number (this corresponds to the send port number on your TouchOSC host). Finally, enter the "CSI sends to port" number (this corresponds to the receive port number in your OSC host application).
9. Click ok to save the device and repeat as needed for additional devices.
10. In the "Pages" section of the CSI Setup screen, click on "HomePage". For additional information, see [[Pages]].
11. Now, on the "Assignments" section on the right, click "Add" to begin adding the Control Surfaces you just setup to the "HomePage"
12. First, you will select the device name from the dropdown
13. Enter the number of channels on your surface (Example: X-Touch/MCU devices have 8 channels)
14. Enter the Channel Start Position (this is 0 by default, but if you're using multiple surfaces, you would use this to offset the start position on the second surface)
15. Select the Surface (.mst) file that corresponds to your surface*
16. Select the Zone folder that corresponds to your surface*

*In CSI, each [[Page|Pages]] can utilize different .mst files and zone folders. This why you first create the Surfaces, then select the Page, then assign the Surface to each Page and define their behavior on each page.

![CSI Preferences Screen Print](https://i.imgur.com/3gqL16s.png)

**Tip:** The information you entered while setting up CSI for the first time gets stored in [[CSI.ini|CSI.INI]] in your CSI folder. Once you're comfortable, it's sometimes easier to edit this information directly in this file. 

## Installing CSI Updates
When a new version of CSI is released, updating CSI involves no more than downloading the latest version and copying over the .dll or dylib to the appropriate UserPlugins folder with Reaper closed, then launching Reaper. Major version updates may introduce new syntax or change existing syntax. When this occurs, this Wiki will be updated to reflect the latest additions or changes.

If you plan on updating any surface or support files, do **NOT** overwrite your current CSI.ini with the one in the .zip folder. By the same token, do not overwrite any .zon files or zone folders you may have customized unless you are intentionally going back to a stock setup - you will lose any customizations you made.