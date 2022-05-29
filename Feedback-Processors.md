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