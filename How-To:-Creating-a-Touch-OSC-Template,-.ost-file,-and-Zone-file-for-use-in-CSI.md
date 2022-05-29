## Creating a Simple OSC Template and .OST
Here, I’m going to show you how to create a simple single channel setup for a phone using TouchOSC Editor, that we’ll then create an .ost and .zon files for use in CSI. The goal of this guide is not to create the best TouchOSC layout ever, but rather, just expose you to the concepts so you can create your own TouchOSC devices in the future by expanding on what you learn here.

## Pre-Conditions:
* You’ve got TouchOSC Editor installed on your computer
* You’ve got TouchOSC installed on your phone or tablet
* They’re all talking to each other and Reaper (see my [[other guide|Setting Up Your Phone or Tablet as an OSC Device in CSI]] on setting up a similar configuration)

## Step 1: Create the Template in TouchOSC Editor
Let’s create a very simple template in Touch OSC Editor. I want a remote control where I can record myself remotely, and use my phone for Play, Record, Stop, with Volume and Pan on the selected channel. I also want a display to show me the Selected Track Name. So 6 controls total.

![Sample CSI Phone Remote](https://i.imgur.com/R7oAZss.png)

1.	Open TouchOSC Editor
2.	Where it says Layout on the left, I’m going to keep the size at “iPhone/iPadTouch” with a vertical orientation (default settings) – You should pick the size and layout that best matches your device!!!
3.	Below that where it says Page, then Name, pick a name for your page. It’s a best practice to name your pages.
4.	If you want to make that name visible on the template, click on “Label” and you can enter the page name in the Text field, and pick a color.
5.	Now, right click on the black background that represents the phone screen. This opens a dropdown menu of available controls/displays/etc. 
6.	Select “Push Button”
7.	Drag that button towards the bottom right and resize it to your liking being careful not to make it so small the control will be difficult to operate. 
8.	With the button selected and highlighted, let’s change the color to Green. Check the box for Local Feedback Off. 
9.	Now, with the button still selected, enter the name “Play” at the top then go down to the OSC tab, uncheck the “Auto” button, and provide a name for this control of: /Play. **Note:** If you were going to use multiple pages on your TouchOSC template, you can include a page number or page name in the control name by using an extra set of slashes. Example: let's say you want to call it page 1, play widget. That would look like this /1/Play. Then on page 2 you wanted a rotary, that might look like /2/Rotary1. And so forth. But let's not do multiple pages and keep it simple with just /Play for now.
 
![CSI Phone Remote Play Button](https://i.imgur.com/JvW7dRN.png)

10.	Now right click a blank area and select “Label H.” 
11.	Drag this label over the middle of our button. 
12.	With the label selected, name this “PlayLabel” at the top-left hand side…
13.	Change the color to Green
14.	Resize it so it fills up most of the button
15.	In the OSC tab uncheck auto
16.	Label this control /PlayLabel
17.	Uncheck the Background box
18.	Where it says “Text” enter Play (this is what will appear in the label unless we tell CSI to show something else)
19.	Change the size to something nice and big (say 20 or above)
 
![CSI Phone Remote Play Button Label](https://i.imgur.com/c6E5iTV.png)

20.	You’ve now created a button and a label
21.	Let’s repeat these steps and create a Stop button (yellow) and a Record button (red) that we’ll put right above the Play button. We’re going to update all of our labels and locations to say Stop and Record respectively. You should end up with something like this.

![CSI Phone Remote Play, Stop and Record Buttons](https://i.imgur.com/RLvfKCF.png)
 
Now let’s add a Fader for volume, a Rotary for pan, and a Label for our Track Name!

22.	Now Right-Click the black area again and select “Fader V” from the dropdown
23.	Drag this to the left of our buttons and make the size something comfortable
24.	Name this Fader1, maybe make it blue, with an OSC control name of /Fader1 – all other settings at default
25.	If the thick part of the fader is at the top, then check the box to invert the faders. For some reason, inverted faders (drawbar style) appear to be the normal.

![CSI Phone Remote Fader Sample](https://i.imgur.com/pzXOhCE.png)

26.	Right click an empty space and select “Rotary V” 
27.	Move it above record and resize it
28.	Let’s also make this blue, name it Rotary1, in the OSC tab give the control a name of /Rotary1 and leave all other settings at default

![CSI Phone Remote Rotary Sample](https://i.imgur.com/GHyxU7R.png)
 
One more control to go, let’s add a label.

29.	Right-click an empty area and select “Label H” again
30.	Let’s center this at the top
31.	Resize it to make wider
32.	Make the font size 18 or so
33.	Label this “DisplayUpper1” with an OSC control name of /DisplayUpper1 and in the Text field lets add the text “Selected Track Name” [optional].
 

![Completed Sample](https://i.imgur.com/YTJzeU8.png)
Do you have something that looks like this?

34.	Now save your template! 

## Step 2. Sync Your New Template to Your Phone or Tablet

1.	In TouchOSC Editor click Sync
2.	Open TouchOSC on your phone or Tablet
3.	Find the layout selection and click the name of your currently selected layout
4.	From the next window that appears, click Add
5.	Wait until your PC appears under “Found Hosts” and click that PC name
6.	This should automatically download your template to your phone or tablet
7.	Go back to the Layout and select it
8.	Click Done in the top right
9.	You should now see your layout on your phone or tablet

## Step 3. Create a CSI .ost File for Your Template
So now we need to define for CSI what controls exist in our template. So just as we need an .mst file for MIDI hardware, we need an .ost file for OSC templates. Being that we just created an OSC template from scratch, let’s create an .ost file from scratch too!

•	Open a text editor to begin creating your .ost file

•	Just like an .mst file, the first line is the word Widget followed by a description of that widget.

•	If it’s a control you’re going to go down a row and enter Control followed by the /[OSC_Control_Name] from our template. So our Play button would be /Play

•	If it’s a Feedback Processor (i.e. you need two-way communication) you need to define that. Controls usually have FB_Processors, but labels can be FB_Processors with no Control. You’re going to enter FB_Processor followed by the /[OSC_Control_Name] from our template. So our Play button would be /Play

•	The last row for each widget simply says WidgetEnd

•	Save your new .ost file in your CSI\Surfaces\OSC folder

Here’s what I’ve setup. Notice that I didn’t create widgets for the labels for Play, Record, and Stop. That’s because I want that text to be fixed and static. 
```
Widget DisplayUpper1
FB_Processor /DisplayUpper1
WidgetEnd

Widget Fader1
Control /Fader1
FB_Processor /Fader1
WidgetEnd

Widget Rotary1
Control /Rotary1
FB_Processor /Rotary1
WidgetEnd

Widget Play
Control /Play
FB_Processor /Play
WidgetEnd

Widget Stop
Control /Stop
FB_Processor /Stop
WidgetEnd

Widget Record
Control /Record
FB_Processor /Record
WidgetEnd

```

## Step 4. Create a Zone Folder and .zon Files
Now we need to tell CSI what to do with this new device by creating a Zone folder and .zon files.

•	Create a new sub-folder under CSI\Zones for your device (Example: CSI\Zones\TouchOSC Remote)

•	Open up a Text Editor

•	Use standard CSI syntax for creating zone files 

Your .zon files should look like this -- they are shown together below but each Zone is in its own file …
```
Zone Home
     IncludedZones
          "Buttons"
          "SelectedChannel"
     IncludedZonesEnd
ZoneEnd


Zone "Buttons"
     Play		Play
     Record	Record
     Stop		Stop
ZoneEnd

Zone "SelectedChannel"
     SelectedTrackNavigator
     Fader1                    TrackVolume
     Rotary1                   TrackPan
     DisplayUpper1       TrackNameDisplay
ZoneEnd
```

## Step 5: Add Your Device to Your CSI Config and Restart Reaper

1.	Open up Reaper’s Preferences
2.	Go to Control/OSC/Web
3.	Click on “Control Surface Integrator” (if not already there follow the installation instructions elsewhere in this wiki)
4.	Click Edit to open the CSI preferences
5.	Click the Page you’d like to add this surface to
6.	Click Add OSC
7.	Follow the instructions for adding an OSC device located elsewhere in this wiki
8.	Click ok to save and apply your changes
9.	Restart Reaper (OSC additions require a full restart)

 
