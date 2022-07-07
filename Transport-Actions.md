The Transport Actions are fairly simple, and as a result, I often use them as "proof of life" actions when I'm first setting up a new surface mapping. If I can get a button on a surface mapped to the Play action, then I know I've got most of the basic setup for the surface working properly and I can move on to doing more advanced things. 

The Transport Actions are:
* Rewind
* FastForward
* Play
* Stop
* Record
* Loop
* [[MCUTimeDisplay|Transport-Actions#MCUTimeDisplay
* [[CycleTimeDisplayModes|Transport-Actions#CycleTimeDisplayModes]]

Little explanation needed for the above actions. They map very directly to the buttons on Reaper's transport controls. 

They take no parameters, so if you want to map a Press widget to Play, you would put the following in your [[zone|Zones]] definition:

`PlayButton Play`

where PlayButton is the name of your widget. 

## MCUTimeDisplay
When paired with the appropriate display widget, the TimeDisplay action will display the time from Reaper, based on the time display mode.

Here's what the time display widget from an MCU device would look like in the mcu.mst file.
```
Widget TimeDisplay
	FB_MCUTimeDisplay
WidgetEnd
````

Then that would be paired with the below action in the .zon file.
```
    TimeDisplay                 MCUTimeDisplay
```

## CycleTimeDisplayModes
This action will change the time display mode in Reaper and surface.

```
    smpteBeats                  CycleTimeDisplayModes
```
