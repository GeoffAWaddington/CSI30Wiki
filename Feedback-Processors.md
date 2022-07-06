Feedback Processors allow you to define how your Widget will display feedback from CSI to the user. A simple example may be lighting up an LED to indicate the Mute/Solo state of a track. Another may be positioning a motorised fader to the correct position for Track Volume. 

It is important to understand how this works in practise.

Let's say we have a button defined in our [[MST file|Defining Control Surface Capabilities]] that has an LED light to indicate its status.    
```
Widget UpperButton1 
  Press 90 20 7f
  FB_TwoState  90 20 7f  90 20 00
WidgetEnd
```

Then in our [[Zone file|Defining Control Surface Behaviour]], we bind it to our [[TrackMute]] action.     
```UpperButton1 TrackMute```

Here's the best case flow of what happens when we press the button on the surface:
* Press the button
* The surface sends the 90 20 7f message to CSI
* CSI attempts to mute the track in Reaper
* CSI then queries Reaper to see if the track is currently muted (eg. if the attempt to mute it worked)
* Assuming it is muted, it then sends the 90 20 7f message back to the surface, instructing it to turn the light on

At some point in the future, the track becomes unmuted (either by pressing the button again as above, or through the Reaper GUI)

* CSI sends the 90 20 00 message back to the surface, instructing it to turn the light off

### Note:
* It's important to understand it's not the surface deciding when to turn the light on or off. It's doing it in response to a message from CSI. We don't want the surface managing the state of the buttons, that responsibility belongs to Reaper/CSI.

## Included Feedback Processors 

* [[FB_TwoState]]

* [[FB_ Fader14Bit]]
* [[FB_ Fader7Bit]]
* [[FB_ Encoder]]
* [[FB_VUMeter]] 
* [[FB_GainReductionMeter]] 
* [[FB_QConProXMasterVUMeter]] 
* [[FB_MCUTimeDisplay]] 
* [[FB_MCUVUMeter]] 
* [[FB_MCUDisplayUpper]] 
* [[FB_MCUDisplayLower]] 
* [[FB_MCUXTDisplayUpper]] 
* [[FB_MCUXTDisplayLower]] 
* [[FB_MCUC4DisplayUpper]] 
* [[FB_MCUC4DisplayLower]]
* [[FB_FP8Display|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP16Display|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP8DisplayUpper|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP16DisplayUpper|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP8DisplayUpperMiddle|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP16DisplayUpperMiddle|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP8DisplayLowerMiddle|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP16DisplayLowerMiddle|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP8DisplayLower|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_FP16DisplayLower|FaderPort8-And-FaderPort16-Feedback-Displays]]
* [[FB_QConLiteDisplayUpper]]
* [[FB_QConLiteDisplayUpperMid]]
* [[FB_QConLiteDisplayLowerMid]]
* [[FB_QConLiteDisplayLower]]
* [[FB_MFT_RGB]]
* [[FB_NovationLaunchpadMiniRGB7Bit]]
* [[FB_FaderportRGB7Bit]]

# FB_MFT (Feedback MIDI Fighter Twister)
This feedback processor allows you to send specific color values to the **MIDI Fighter Twister** (MFTwister for short), and the information in this page is only applicable to this device.

Note: Within CSI, only the buttons on the MFTwister can be colored, not the encoder rings themselves. This means the below section will only be applicable to the button/press widgets. 

## Adding Color Feedback To Your Widgets in the .mst File
Let's say you've got a button widget called ButtonA1 on your MFTwister, and it's at the address b1 00 7f. If you would like to take control of the button color within CSI, you would first open the .mst file and setup your widget to use the following format:

```` 
Widget ButtonA1
    Press b1 00 7f
    FB_MFT_RGB  b1 00 7f
WidgetEnd
```` 

Do this for any buttons you would like to control the colors for. This just tells CSI that the colors for this button can be controlled within CSI. We will define the color values that get linked to a particular action in the .zon files so continue reading below.

## Adding Colors to Your .Zon Files/Actions
**Background:** the MFTwister color settings are a little tricky. From Geoff, "the RGB table is such that any one colour (r, g, b) MUST be 0 AND any OTHER colour (r, g, b) MUST be 255 -- the third colour can vary in the range 1 - 255 or so -- the colour table is a bit quirky." So for any state (i.e. off/on), two of the 3 colors must be 0 and 255. 

Following your action, colors must appear in curly brackets following the action and require a space after the first bracket and before the last one. The first 3 numbers in the brackets are the RGB values for the off state. The second 3 numbers are the RGB values for the on state.

In this example, the color will change from deep blue (off), to light blue (on) based on the Playback state in Reaper.
```` 
Zone "Buttons|"
        ButtonB8 Play { 0 25 255 0 189 255 }
ZoneEnd
```` 

In this example, from an FX.zon, the MFTwister button will turn green when the effect is on, and red when bypassed. If you wanted green in the off state and red in the on state, you'd flip the order of the first-three numbers and second-three.
````
Zone "VST: AR-1 (Kush Audio)"
FocusedFXNavigator 
Toggle+ButtonB8 FXParam 0 "Bypass" { 90 255 0 255 50 0 } 
ZoneEnd
```` 

## Tip: Using Dummy Colors in FX.zon Files
Let's say our fx.zon includes some unmapped encoders and you want to give yourself some visual feedback that says, "hey, ButtonsB1 through ButtonsB3 have nothing mapped to them." You could setup your fx.zon files to include a color for this. This way, when you see the color, you know there's nothing mapped to that parameter. 

For example, here I'm a dummy FX parameter (parameter 999 doesn't exist for this plugin) combined with Navy Blue to indicate there's nothing mapped to an encoder or button:

```` 
ButtonB1	FXParam 999 "Dummy" { 0 25 255 0 25 255 }
ButtonB2	FXParam 999 "Dummy" { 0 25 255 0 25 255 }	
ButtonB3	FXParam 999 "Dummy" { 0 25 255 0 25 255 }	
```` 

Now, let's say I want the light to turn green under any mapped rotary encoders. You can use the same method as above, when combined with your normal widget action. Example: here, RotaryA2 is mapped to a stepped ratio control of a plugin. At the lowest ratio, the encoder rings don't light up. So how would I know that encoder is mapped to something? Well, I can light the button below it green by doing the following:
```` 
RotaryA2	FXParam 5 "Ratio"	[ 0.0 0.34 0.67 1.0 ] //these are the parameter steps not the colors
ButtonA2	FXParam 999 "Dummy" { 90 255 0 90 255 0 }
```` 

## FB_MFT_RGB Color Reference Table
Lastly, to save you the time and effort, here are approximations of the MIDI Fighter Twister Utility's color codes, adapted for use in CSI.

```` 
	               R	G	B
Navy Blue	       0	75	255
Dodger Blue	       0	145	255
Deep Sky Blue	       0	215	255
Aqua	               0	250	255
Spring Green	       0	255	135
Lime Green	       25	255	0
Bright Green	       90	255	0
Spring Bud	       165	255	0
Yellow	               242	255	0
Selective Yellow       255	188	0
Safety Orange	       255	115	0
Scarlet Red	       255	50	0
Tarch Red	       255	0	80
Hot Magenta	       255	0	225
Magenta	               255	0	255
Psychedelic Purple     240	0	255
Electric Purple	       165	0	255
```` 

## FB_QConProXMasterVUMeter (Feedback QCon ProX Master VU Meter)
Use this widget for Qcon Pro X|iCON master meters. 

If you have one of these surfaces, your widgets should look like this in the .mst:

```` 
Widget MasterChannelMeterLeft
	FB_QConProXMasterVUMeter
WidgetEnd

Widget MasterChannelMeterRight
	FB_QConProXMasterVUMeter
WidgetEnd
```` 

Also, make sure you specify Left (0) and Right (1) in the.zon file.

```` 
Zone "MasterTrack"
MasterTRackNavigator
    MasterChannelMeterLeft TrackOutputMeter  0
    MasterChannelMeterRight TrackOutputMeter  1
ZoneEnd
```` 

# FaderPort8 and FaderPort16 Displays
CSI supports the Presonus FaderPort8 and FaderPort16 Displays. Each of the 4 display rows requires it's own widget in the .mst file.

For your .mst, here are the names for the FB generators that correspond to each line on the surface. 

```
FB_FP8DisplayUpper

FB_FP8DisplayUpperMiddle

FB_FP8DisplayLowerMiddle

FB_FP8DisplayLower // DisplayLower not implemented as of Dec 18 2020 - do not use.



FB_FP16DisplayUpper

FB_FP16DisplayUpperMiddle

FB_FP16DisplayLowerMiddle

FB_FP16DisplayLower // DisplayLower not implemented as of Dec 18 2020 - do not use.
```

And here is an example of how the .mst would be mapped out for Display 1 on the Faderport16.
```
Widget FPScribbleUpper1
	FB_FP16DisplayUpper "0"
WidgetEnd

Widget FPScribbleUpperMiddle1
	FB_FP16DisplayUpperMiddle "0"
WidgetEnd

Widget FPScribbleLowerMiddle1
	FB_FP16DisplayLowerMiddle "0"
WidgetEnd

Widget FPScribbleLower1
	FB_FP16DisplayLower "0"
WidgetEnd
```

**Note:** FB_FP8Display and FB_FP16Display are legacy feedback processors that now correspond to FB_FP8DisplayUpper and FB_FP16DisplayUpper respectively. The legacy versions will continue working for any .mst files where they already exist, but if you're creating a new set of files, you are encouraged to use the newer feedback processors.