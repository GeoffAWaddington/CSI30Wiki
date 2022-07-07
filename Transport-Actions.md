The Transport Actions are fairly simple, and as a result, I often use them as "proof of life" actions when I'm first setting up a new surface mapping. If I can get a button on a surface mapped to the Play action, then I know I've got most of the basic setup for the surface working properly and I can move on to doing more advanced things. 

The Transport Actions are:
* [[Rewind|Transport-Actions#Rewind]]
* [[FastForward|Transport-Actions#FastForward]]
* [[Play|Transport-Actions#Play]]
* [[Stop|Transport-Actions#Stop]]
* [[Record|Transport-Actions#Record]]
* [[CycleTimeline|Transport-Actions#CycleTimeline]]
* [[MCUTimeDisplay|Transport-Actions#MCUTimeDisplay]]
* [[CycleTimeDisplayModes|Transport-Actions#CycleTimeDisplayModes]]

## Rewind
Rewind moves the Reaper Edit/Play cursor. The Rewind action in CSI has a latching behavior where a single button press starts rewinding until you press stop. A second press of Rewind causes it to Rewind at a faster speed. 

```
Zone "Buttons"
   Rewind     Rewind
ZoneEnd
```

## FastForward
FastForward moves the Reaper Edit/Play cursor. The FastForward action in CSI has a latching behavior where a single button press starts fast-forwarding until you press stop. A second press of FastForward causes it to FastForward at a faster speed. 

```
Zone "Buttons"
   FastForward     FastForward
ZoneEnd
```

## Play
Begins playback in Reaper.

```
Zone "Buttons"
    Play     Play
ZoneEnd
```

## Stop
Stops playback in Reaper.

```
Zone "Buttons"
    Stop     Stop
ZoneEnd
```

## CycleTimeline
Engages Reaper's "Toggle Repeat" (aka "Loop") mode.

```
Zone "Buttons"
    Cycle     CycleTimeline
ZoneEnd
```

## MCUTimeDisplay
When paired with the appropriate FB_MCUTimeDisplay widget, the TimeDisplay action will display the time from Reaper, based on the time display mode.

Then that would be paired with the below action in the .zon file.
```
    TimeDisplay                 MCUTimeDisplay
```

## CycleTimeDisplayModes
This action will change the time display mode in Reaper and surface.

```
    smpteBeats                  CycleTimeDisplayModes
```
