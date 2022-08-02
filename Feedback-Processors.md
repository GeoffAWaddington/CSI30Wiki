Feedback Processors allow you to define how your Widget will display feedback from CSI to the user. A simple example may be lighting up an LED to indicate the Mute/Solo state of a track. Another may be positioning a motorised fader to the correct position for Track Volume. 

It is important to understand how this works in practise.

Let's say we have a button defined in our [[MST file|Defining Control Surface Capabilities]] that has an LED light to indicate its status.    
```
Widget Mute1 
  Press 90 20 7f
  FB_TwoState 90 20 7f 90 20 00
WidgetEnd
```

Then in our [[Zone file|Defining Control Surface Behaviour]], we bind it to our [[TrackMute|Track Actions#TrackMute]] action.     
```
Zone "SelectedTrack"
     Mute1      TrackMute
ZoneEnd
```

Here's the best-case flow of what happens when we press the button on the surface:
* Press the Mute button
* The surface sends the 90 20 7f message to CSI
* CSI attempts to mute the track in Reaper
* CSI then queries Reaper to see if the track is currently muted (eg. if the attempt to mute it worked)
* Assuming it is muted, it then sends the 90 20 7f message back to the surface, instructing it to turn the light on

At some point in the future, the track becomes unmuted (either by pressing the button again as above, or through the Reaper GUI)

* CSI sends the 90 20 00 message back to the surface, instructing it to turn the light off

**Note:** It's important to understand it's not the surface deciding when to turn the light on or off. It's doing it in response to a message from CSI. We don't want the surface managing the state of the buttons, that responsibility belongs to Reaper/CSI.

# Feedback Processors 

* [[FB_TwoState|Feedback-Processors#FB_TwoState]]
* [[FB_Fader14Bit|Feedback-Processors#FB_Fader14Bit]]
* [[FB_Fader7Bit|Feedback-Processors#FB_Fader7Bit]]
* [[FB_Encoder|Feedback-Processors#FB_Encoder]]
* [[FB_GainReductionMeter|Feedback-Processors#FB_GainReductionMeter]] - As of Aug 2, 2022, being renamed FB_ConsoleOneVUMeter
* [[FB_VUMeter|Feedback-Processors#FB_VUMeter]] - As of Aug 2, 2022, being renamed FB_ConsoleOneVUMeter
* [[FB_MCUDisplayUpper|Feedback-Processors#FB_MCUDisplayUpper]] 
* [[FB_MCUDisplayLower|Feedback-Processors#FB_MCUDisplayLower]] 
* [[FB_MCUXTDisplayUpper|Feedback-Processors#FB_MCUXTDisplayUpper]] 
* [[FB_MCUXTDisplayLower|Feedback-Processors#FB_MCUXTDisplayLower]] 
* [[FB_MCUC4DisplayUpper|Feedback-Processors#FB_MCUC4DisplayUpper]] 
* [[FB_MCUC4DisplayLower|Feedback-Processors#FB_MCUC4DisplayLower]]
* [[FB_MCUAssigmentDisplay|Feedback-Processors#FB_MCUAssigmentDisplay]]
* [[FB_MCUTimeDisplay|Feedback-Processors#FB_MCUTimeDisplay]] 
* [[FB_MCUVUMeter|Feedback-Processors#FB_MCUVUMeter]] 
* [[FB_FaderportRGB7Bit|Feedback-Processors#fb_faderportrgb7bit]]
* [[FB_FP8DisplayUpper|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP16DisplayUpper|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP8DisplayUpperMiddle|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP16DisplayUpperMiddle|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP8DisplayLowerMiddle|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP16DisplayLowerMiddle|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP8DisplayLower|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP16DisplayLower|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP8Display|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_FP16Display|Feedback-Processors#faderport8-and-faderport16-displays]]
* [[FB_MFT_RGB|Feedback-Processors#FB_MFT_RGB]]
* [[FB_NovationLaunchpadMiniRGB7Bit|Feedback-Processors#fb_novationlaunchpadminirgb7bit]]
* [[FB_QConLiteDisplayUpper|Feedback-Processors#qcon-lite-displays]]
* [[FB_QConLiteDisplayUpperMid|Feedback-Processors#qcon-lite-displays]]
* [[FB_QConLiteDisplayLowerMid|Feedback-Processors#qcon-lite-displays]]
* [[FB_QConLiteDisplayLower|Feedback-Processors#qcon-lite-displays]]
* [[FB_QConProXMasterVUMeter|Feedback-Processors#FB_QConProXMasterVUMeter]] 
* [[FB_XTouchDisplayUpper|Feedback-Processors#FB_XTouchDisplayUpper]] - CSI Exp only as of July 31, 2022

## FB_TwoState
FB_TwoState is designed to provide feedback to buttons that have only an on or off state. Here's an example of the Mute button on channel 1 of an MCU style device. Notice it consists of a "Press" portion (the message generator for this widget), and also the FB_TwoState portion. The messages that follow are the bytes that are transmitted to control the state.
```
Widget Mute1
	Press 90 10 7f 90 10 00
	FB_TwoState 90 10 7f 90 10 00
WidgetEnd
```

Note: if a button is assigned to a Reaper action, you will only see feedback if the action reports its on/off state to Reaper. You will see this in the Reaper action list where applicable. Otherwise, a light may appear in a constant on or off state. This is not a bug in CSI.

## FB_Fader14Bit
Use this feedback processor for 14-bit motorized faders, such an MCU-style device, or any other MIDI control surface with 14-bit knobs or faders. Below is an example of a touch-sensitive 14-bit fader that you'd see in an MCU-style device.
```
Widget Fader1
	Fader14Bit e0 7f 7f
	FB_Fader14Bit e0 7f 7f
	Touch 90 68 7f 90 68 00
WidgetEnd
```

## FB_Fader7Bit
Use Fader7Bit for any traditional "absolute" [as in having a fixed start and end point] MIDI faders and knobs (yes, Fader7Bit applies to knobs too!). Here's the syntax for a typical 7-bit MIDI knob with feedback. 
```
Widget Knob1
	Fader7Bit B0 16 7F
	FB_Fader7Bit B0 16 00
WidgetEnd
```

Note: some surfaces, like the MIDI Fighter Twister, use encoders but may have 7Bit LED rings to represent the value of the current parameter, so you may end up with an Encoder widget with a Fader7Bit feedback processor. 
```
Widget RotaryA1
	MFTEncoder b0 00 7f
	FB_Fader7Bit b0 00 00
WidgetEnd
```

## FB_Encoder
Use FB_Encoder for surfaces with a continuous encoder (no absolute start and end point) and feedback that expects the same.
```
Widget Rotary1
	Encoder b0 10 7f [ < 41 > 01 ]
	FB_Encoder b0 10 00
WidgetEnd
```

## FB_GainReductionMeter
Use FB_GainReductionMeter for the gain reduction meter on the Softube Console One.
```
Widget CompressorMeter
	FB_GainReductionMeter b0 73 00
WidgetEnd
```

## FB_VUMeter 
FB_VUMeter is designed for the meters in the Softube Console One. An example of these definitions is shown below.
```
Widget InputMeterLeft
	FB_VUMeter b0 6e 00
WidgetEnd

Widget InputMeterRight
	FB_VUMeter b0 6f 00
WidgetEnd

Widget OutputMeterLeft
	FB_VUMeter b0 70 00
WidgetEnd

Widget OutputMeterRight
	FB_VUMeter b0 71 00
WidgetEnd
```

## FB_MCUDisplayUpper
This is the feedback processor used for the upper displays on an MCU-style surface (see [[FB_XTouchDisplayUpper]] if using an X-Touch or X-Touch Extender). The syntax is FB_MCUDisplayUpper followed by the channel # beginning with 0 for channel 1. Here's an example from an 8-channel MCU surface.

```
Widget DisplayUpper1
	FB_MCUDisplayUpper 0
WidgetEnd

Widget DisplayUpper2
	FB_MCUDisplayUpper 1
WidgetEnd

Widget DisplayUpper3
	FB_MCUDisplayUpper 2
WidgetEnd

Widget DisplayUpper4
	FB_MCUDisplayUpper 3
WidgetEnd

Widget DisplayUpper5
	FB_MCUDisplayUpper 4
WidgetEnd

Widget DisplayUpper6
	FB_MCUDisplayUpper 5
WidgetEnd

Widget DisplayUpper7
	FB_MCUDisplayUpper 6
WidgetEnd

Widget DisplayUpper8
	FB_MCUDisplayUpper 7
WidgetEnd
```

## FB_MCUDisplayLower
Similar to [[FB_MCUDisplayUpper|Feedback-Processors#FB_MCUDisplayUpper]], except covers the lower displays of MCU-style surfaces.
```
Widget DisplayLower1
	FB_MCUDisplayLower 0
WidgetEnd

Widget DisplayLower2
	FB_MCUDisplayLower 1
WidgetEnd

Widget DisplayLower3
	FB_MCUDisplayLower 2
WidgetEnd

Widget DisplayLower4
	FB_MCUDisplayLower 3
WidgetEnd

Widget DisplayLower5
	FB_MCUDisplayLower 4
WidgetEnd

Widget DisplayLower6
	FB_MCUDisplayLower 5
WidgetEnd

Widget DisplayLower7
	FB_MCUDisplayLower 6
WidgetEnd

Widget DisplayLower8
	FB_MCUDisplayLower 7
WidgetEnd
```

## FB_MCUXTDisplayUpper
Similar to [[FB_MCUDisplayUpper|Feedback-Processors#FB_MCUDisplayUpper]], except covers the upper displays of MCU Extender-style (XT) surfaces.
```
Widget DisplayUpper1
	FB_MCUXTDisplayUpper 0
WidgetEnd

Widget DisplayUpper2
	FB_MCUXTDisplayUpper 1
WidgetEnd

Widget DisplayUpper3
	FB_MCUXTDisplayUpper 2
WidgetEnd

Widget DisplayUpper4
	FB_MCUXTDisplayUpper 3
WidgetEnd

Widget DisplayUpper5
	FB_MCUXTDisplayUpper 4
WidgetEnd

Widget DisplayUpper6
	FB_MCUXTDisplayUpper 5
WidgetEnd

Widget DisplayUpper7
	FB_MCUXTDisplayUpper 6
WidgetEnd

Widget DisplayUpper8
	FB_MCUXTDisplayUpper 7
WidgetEnd
```

## FB_MCUXTDisplayLower
Similar to [[FB_MCUDisplayUpper|Feedback-Processors#FB_MCUDisplayUpper]], except covers the lower displays of MCU Extender-style (XT) surfaces.
```
Widget DisplayLower1
	FB_MCUXTDisplayLower 0
WidgetEnd

Widget DisplayLower2
	FB_MCUXTDisplayLower 1
WidgetEnd

Widget DisplayLower3
	FB_MCUXTDisplayLower 2
WidgetEnd

Widget DisplayLower4
	FB_MCUXTDisplayLower 3
WidgetEnd

Widget DisplayLower5
	FB_MCUXTDisplayLower 4
WidgetEnd

Widget DisplayLower6
	FB_MCUXTDisplayLower 5
WidgetEnd

Widget DisplayLower7
	FB_MCUXTDisplayLower 6
WidgetEnd

Widget DisplayLower8
	FB_MCUXTDisplayLower 7
WidgetEnd
```

## FB_MCUC4DisplayUpper
Similar to [[FB_MCUDisplayUpper|Feedback-Processors#FB_MCUDisplayUpper]], except covers the upper displays of the MCU C4 surface.
```
Widget DisplayUpper1
	FB_MCUC4DisplayUpper 0
WidgetEnd

Widget DisplayUpper2
	FB_MCUC4DisplayUpper 1
WidgetEnd

Widget DisplayUpper3
	FB_MCUC4DisplayUpper 2
WidgetEnd

Widget DisplayUpper4
	FB_MCUC4DisplayUpper 3
WidgetEnd

Widget DisplayUpper5
	FB_MCUC4DisplayUpper 4
WidgetEnd

Widget DisplayUpper6
	FB_MCUC4DisplayUpper 5
WidgetEnd

Widget DisplayUpper7
	FB_MCUC4DisplayUpper 6
WidgetEnd

Widget DisplayUpper8
	FB_MCUC4DisplayUpper 7
WidgetEnd
```

## FB_MCUC4DisplayLower
Similar to [[FB_MCUDisplayUpper|Feedback-Processors#FB_MCUDisplayUpper]], except covers the lower displays of the MCU C4 surface.
```
Widget DisplayLower1
	FB_MCUC4DisplayLower 0
WidgetEnd

Widget DisplayLower2
	FB_MCUC4DisplayLower 1
WidgetEnd

Widget DisplayLower3
	FB_MCUC4DisplayLower 2
WidgetEnd

Widget DisplayLower4
	FB_MCUC4DisplayLower 3
WidgetEnd

Widget DisplayLower5
	FB_MCUC4DisplayLower 4
WidgetEnd

Widget DisplayLower6
	FB_MCUC4DisplayLower 5
WidgetEnd

Widget DisplayLower7
	FB_MCUC4DisplayLower 6
WidgetEnd

Widget DisplayLower8
	FB_MCUC4DisplayLower 7
WidgetEnd
```

## FB_MCUAssigmentDisplay
On an MCU or X-Touch it will display the overall 'mode' CSI is currently in, on the LED display labelled 'Assignment' immediately to then left of the SMPTE/Beats indicators (on an X-Touch, it's immediately to the left of the master solo indicator)

The Modes are either Normal, ie regular track display; VCA, VCA leaders; or Folder, top level folders.

The widget would look like this:
```
Widget AssigmentDisplay
	FB_MCUAssigmentDisplay
WidgetEnd
```

The zone would look like this:
```
Zone "Buttons"
    AssignmentDisplay         TrackVCAFolderModeDisplay
ZoneEnd
```

## FB_MCUTimeDisplay
FB_MCUTimeDisplay will display the time in Reaper, according to whichever mode Reaper is currently set to display time in. The below example shows what the MCU time display widget would look like in the mcu.mst file.
```
Widget TimeDisplay
	FB_MCUTimeDisplay
WidgetEnd
````

## FB_MCUVUMeter 
Use FB_MCUVUMeter for the VU meters on an MCU-style device. The syntax for this type of processor is FB_MCUVUMeter followed by the channel number (starting at 0 for channel 1). So an 8-channel surface would look like the below example.
```
Widget VUMeter1
	FB_MCUVUMeter 0
WidgetEnd

Widget VUMeter2
	FB_MCUVUMeter 1
WidgetEnd

Widget VUMeter3
	FB_MCUVUMeter 2
WidgetEnd

Widget VUMeter4
	FB_MCUVUMeter 3
WidgetEnd

Widget VUMeter5
	FB_MCUVUMeter 4
WidgetEnd

Widget VUMeter6
	FB_MCUVUMeter 5
WidgetEnd

Widget VUMeter7
	FB_MCUVUMeter 6
WidgetEnd

Widget VUMeter8
	FB_MCUVUMeter 7
WidgetEnd
```

## FB_MFT_RGB
This feedback processor allows you to send specific color values to the **MIDI Fighter Twister** (MFTwister for short). Note: Within CSI, only the buttons on the MFTwister can be colored, not the encoder rings themselves. This means the below section will only be applicable to the button/press widgets. 

Let's say you've got a button widget called ButtonA1 on your MFTwister, and it's at the address b1 00 7f. If you would like to take control of the button color within CSI, you would add the FB_MFT_RGB feedback processor using the same address as the button itself as shown in the below example.
```` 
Widget ButtonA1
    Press b1 00 7f
    FB_MFT_RGB b1 00 7f
WidgetEnd
```` 

Do this for any buttons you would like to control the colors for. This just tells CSI that the colors for this button can be controlled within CSI. We will define the color values that get linked to a particular action in the .zon files so continue reading below.

### Adding Colors to Your .Zon Files/Actions
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

### Tip: Using Dummy Colors in FX.zon Files
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

### FB_MFT_RGB Color Reference Table
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

## FB_FaderportRGB7Bit 
Use FB_FaderportRGB7Bit for controlling the RGB buttons on a Faderport8 or Faderport16 device. See the examples below.
```
Widget AudioBtn
	Press 90 3e 7f
	FB_FaderportRGB7Bit 90 3e 7f
WidgetEnd

Widget Instrument
	Press 90 3f 7f
	FB_FaderportRGB7Bit 90 3f 7f
WidgetEnd

Widget BusBtn
	Press 90 40 7f
	FB_FaderportRGB7Bit 90 40 7f
WidgetEnd

Widget VCABtn
	Press 90 41 7f
	FB_FaderportRGB7Bit 90 41 7f
WidgetEnd

Widget AllBtn
	Press 90 42 7f
	FB_FaderportRGB7Bit 90 42 7f
WidgetEnd
```

## FaderPort8 and FaderPort16 Displays
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

## FB_NovationLaunchpadMiniRGB7Bit
Use FB_NovationLaunchpadMiniRGB7Bit for controlling the RGB colors on the Novation Launchpad Mini buttons. See the example below.
```
Widget ButtonA1
        Press b0 5b 7f
        FB_NovationLaunchpadMiniRGB7Bit b0 5b 7f
WidgetEnd
```

## QCon Lite Displays
Use FB_QConLiteDisplayUpper, FB_QConLiteDisplayUpperMid, FB_QConLiteDisplayLowerMid, FB_QConLiteDisplayLower for the four display lines on the QCon Lite surface. The correct syntax for each includes the Feedback Processor name, followed by the Channel # starting with 0.
```
Widget DisplayUpper1
	FB_QConLiteDisplayUpper 0
WidgetEnd

Widget DisplayUpperMid1
        FB_QConLiteDisplayUpperMid 0
WidgetEnd

Widget DisplayLowerMid1
        FB_QConLiteDisplayLowerMid 0
WidgetEnd

Widget DisplayLower1
	FB_QConLiteDisplayLower 0
WidgetEnd
```

## FB_QConProXMasterVUMeter
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

## FB_XTouchDisplayUpper
FB_XTouchDisplayUpper is used for controlling colors on the displays for the X-Touch Universal and X-Touch Extender controllers. This feedback processor should exist in the UpperDisplay1 widget only and will control the colors for all 8 displays on the unit. The remaining displays should be FB_MCUDisplayUpper as shown in the example below.
```
Widget DisplayUpper1
	FB_XTouchDisplayUpper 0
WidgetEnd

Widget DisplayUpper2
	FB_MCUDisplayUpper 1
WidgetEnd

Widget DisplayUpper3
	FB_MCUDisplayUpper 2
WidgetEnd

Widget DisplayUpper4
	FB_MCUDisplayUpper 3
WidgetEnd

Widget DisplayUpper5
	FB_MCUDisplayUpper 4
WidgetEnd

Widget DisplayUpper6
	FB_MCUDisplayUpper 5
WidgetEnd

Widget DisplayUpper7
	FB_MCUDisplayUpper 6
WidgetEnd

Widget DisplayUpper8
	FB_MCUDisplayUpper 7
WidgetEnd
```

**Important Note:** The X-Touch firmware only supports 8 track colors. For best results use track colors in Reaper that closely correspond to the below colors...
```
Black
White
Red
Green
Blue
Cyan
Magenta
Yellow
```