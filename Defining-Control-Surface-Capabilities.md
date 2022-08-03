If we want to use one or more Control Surfaces with CSI, we need to define what capabilities they have. ie. what controls they have (eg. buttons, faders, encoders, etc), what  feedback mechanisms they have (displays, lights, etc), etc. 

.mst files are for midi devices and are used in the following examples -- similarly, .ost files are for OSC devices -- see the CSI/Surfaces/OSC folder for OSC examples.

Note, this is nothing yet to do with what should happen when those buttons, etc are clicked, we're just defining the capabilities. 


We define these capabilities in a file with a .mst extension. Beyond the .mst extension, the rest of the filename is up to you, but it makes sense for it to reflect the model of controller. So for a BCF2000 control surface, I'd have a BCF2000.mst file, for a Launchpad Mini, I'd probably have a LaunchpadMini.mst. 

Here's a snippet from one of my .mst files:

```     
Widget Rotary8
	Fader7Bit b0 20 7f
	FB_Fader7Bit b0 20 00
        Toggle 90 1f 7f
WidgetEnd

Widget RotaryPush8 
    Press 90 1f 7f
WidgetEnd

Widget UpperButton1 
    Press 90 20 7f
    FB_TwoState 90 20 7f  90 20 00
WidgetEnd 

Widget Fader1 
    Fader14Bit e0 7f 7f
    FB_Fader14Bit e0 7f 7f
    Touch 90 68 7f 90 68 00
WidgetEnd   
```    

A few things to note:
* We're defining named Widgets. A Widget very often maps to a single physical control on a Surface, however it's important to realise it doesn't have to. For example a Widget may be a combination of a button and separate LED.
* Within each Widget, we define the capabilities of that widget. These could be one or more of either:
  * [[Message Generators]] - things that will result in a message being sent to CSI. eg. Press, Fader14Bit, etc
  * [[Feedback Processors]] - things that will receive a message back from CSI to display feedback on the surface. eg. FB_TwoState, FB_Fader14Bit, etc where the FB represents FeedBack. 
* Each of the Message Generators and Feedback Processors may have a varying number of parameters that follow them that define the MIDI Message details sent/received and other behavior. Look at the specific [[Message Generators]]/[[Feedback Processors]] you are using to see what parameters it supports. 

So for example, my UpperButton1 Widget is defined as:
```
Widget UpperButton1 
    Press 90 20 7f
    FB_TwoState 90 20 7f  90 20 00
WidgetEnd 
```
Which says this Widget is a combination of a:
* Press MIDI Generator, which means when pressed it will send the MIDI Message 90 20 7f 
* and a FB_TwoState Feedback Processor, which will:
  * receive a MIDI Message of either 90 20 7f or 90 20 00 and display that to the user. In my case, this is displayed as an LED, where 90 20 7f represents the On state and 90 20 00 represents the Off state. 

Whereas my RotaryPush8 widget is much simpler, using a Press Message Generator that sends only the MIDI Message 90 20 7f when it is pressed. Nothing on release, and no feedback.  
```
Widget RotaryPush8 
    Press 90 1f 7f
WidgetEnd
```


## Note:
* Because the .mst file doesn't specify anything about behavior, just surface capabilities, in theory anyone with the same model surface should be able to reuse the same .mst file, even if they want to assign different behaviors to those controls. eg. If I have a BCF2000, and so do you, in theory we can use the same .mst file. However, be careful here as if you have changed the configuration of your surface, it may be sending different midi messages, in which case someone else's .mst file may not work for you. 
* It's easy to forget that you're not controlling what the surface does in your MST. It is more accurate to say you are reflecting what it does. For example, if my surface isn't currently sending a message on button release, then defining it as a [[Press]] control  with a Release message in your .mst won't magically make it start. Either edit the surface (if possible) to make it do that, or define it in the .mst as a [[Press]] with a single message.