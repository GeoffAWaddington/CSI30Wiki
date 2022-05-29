# Setting Up Your Phone or Tablet as an OSC Device in CSI
CSI is compatible with MIDI, EuCon, and Open Sound Control (OSC) devices. OSC allows you to use a phone or tablet to interact with your CSI setup; essentially acting like another control surface. 

There are OSC compatible applications that you can download on your phone or tablet, and you can create OSC templates that can be programed to interact with CSI like any other program. This guide will focus on setting up the included Mackie C4 emulator OSC template using TouchOSC on an iPad.
The exact instructions may vary from device to device, or depending on the OSC application you're using, but the high-level concepts should be similar regardless of device or application.

## Pre-Condition: 
1.	You’ve already setup CSI according to the Installation Instructions (including the OSC related folders and files)
2.	Your phone or tablet must be on the same network as your PC or Mac

## Step 1: Download TouchOSC On Your Phone or Tablet
First things first, go to the Android or iOS app store and download TouchOSC to the device you’d like to use.

## Step 2: Download and Install the TouchOSC Editor To Your PC or Mac
The TouchOSC Editor can be found here: https://hexler.net/products/touchosc#downloads

## Step 3: Transfer the C4Emu TouchOSC Template to Your Phone or Tablet
CSI comes with some TouchOSC templates, so we’re going to open the C4Emu template in TouchOSC editor, then add it to TouchOSC on our phone or tablet.
1.	Open TouchOSC Editor
2.	Click File -> Open
3.	Navigate to the Reaper resource path folder where you installed CSI
4.	Open the CSI\Touch OSC Layouts\ folder
5.	Locate and open the C4Emu.touchosc file – the file will open
6.	Click the “Sync” button in TouchOSC Editor
7.	Open TouchOSC on your phone or tablet
8.	Under where it says Layout click the current layout name. This will open the Layout selection screen. Note: If you’re already in a layout, click in the dot in the top-right-hand corner to go back. 
9.	In the Layout selection screen, the top row says “Add” click this
10.	Under “Found Hosts” your PC name should appear – click the PC name

You should now have successfully loaded the C4Emu.touchosc template to your device.

## Step 3 : Let’s Find Out Our Computer Host/Local IP Address
The first thing we should do is find out what our Host Computer’s IP address (if you don’t already know). Lucky for us, Reaper can just tell us. So…

1.	Open Reaper’s Preferences -> Control/OSC/WEB
2.	Click Add
3.	Select OSC (Open Sound Control)
4.	In the Mode dropdown select “Configure device IP+local port)
5.	See where it says “Local IP:” followed by a number? Example: 192.1.0.124 That’s your Local IP address. Make note of this.
6.	Click CANCEL – we don’t actually want to create the OSC device in here, we just wanted to know what our IP address was.

## Step 4: Open the TouchOSC App on Your Phone or Tablet and Enter Your IP Address
Here we are going to take the Local IP address from the prior screen, and enter it as the Host IP address in TouchOSC. We’re also going to make note of some device IP details for use later in Reaper/CSI.

1.	Open TouchOSC
2.	Where it says “Connections” select the first row “OSC”
3.	Make sure OSC is enabled
4.	Where it says “Host” enter your Local IP Address from the prior step
5.	Now make note of your “Port Outgoing” (usually 8000)
6.	Make note of your “Port Incoming” (usually 9000)
7.	Make note of the “Local IP Address” (example: 192.1.0.193) – this is the IP address of your phone or tablet
8.	We’ll need these to setup CSI

## Step 5: Create the OSC Device in CSI

1.	Open Reaper’s Preferences -> Control/OSC/WEB
2.	Click on Control Surface Integrator from the list (if it’s not already there, install CSI according to the Installation instructions).
3.	Click Edit – the Control Surface Settings screen should open
4.	On the Pages section on the left, click Home (or whatever page you want to add the OSC device on)
5.	Click the “Add OSC” button
6.	Name your device, example: iPad C4
7.	Enter 8 channels, 8 sends, and 8 FX menus – this will give some flexibility later
8.	Where it says “Remote device IP” this is the IP address of your Phone or Tablet from Step 4.7 above
9.	"CSI Receives On Port" will be the Outgoing Port from Step 4.5 above (8000)
10.	"CSI Sends on Port" will be the Incoming Port from Step 4.6 above (9000)
11.	Where it says surface select MackieC4Emu.ost
12.	Where it says zone select C4Emu
13.	Hit ok to save and apply all changes
14.	Close and restart Reaper. For whatever reason, at least here, OSC changes require a full restart of Reaper.

If everything went according to plan, you’ve just mapped your phone or tablet to act as a control surface in Reaper+CSI.

## Ok, Now what?
All we’ve done so far is connected your phone or tablet to Reaper+CSI. You still have to create .zon files for any FX or Reaper actions you’d like to control. See the rest of the Wiki for guidance on how to go about that.
