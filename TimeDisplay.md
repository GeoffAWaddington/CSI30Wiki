## TimeDisplay
When paired with the appropriate display widget, the TimeDisplay action will display the time from Reaper, based on the time display mode.

Here's what the time display widget from an MCU device would look like in the mcu.mst file.
```
Widget TimeDisplay
	FB_MCUTimeDisplay
WidgetEnd
````

Then that would be paired with the below action in the .zon file.
```
    TimeDisplay                 TimeDisplay
```

## CycleTimeDisplayModes
This action will change the time display mode in Reaper and surface.

```
    smpteBeats                  CycleTimeDisplayModes
```