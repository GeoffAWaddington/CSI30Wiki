## TrackPan
Here is the basic syntax TrackPan assigned to a single Rotary parameter...

```
	Rotary			TrackPan "0"
```

Now, notice the number after TrackPan...

The numbers are for the LED ring displays on the MCU style encoders:

* 0 means a single led -- perfect for Pan
* 1 means fill from right edge - centre single -- fill to left edge perfect for Width or EQ boost/cut
* 2 - left right fill -- good for level
* 3 - spread -- good for Q

### TrackPan + RotaryPush to Switch Between Pan and PanWidth Zones

Use this all-in-one approach if you have a pushtop rotary.

```
 Rotary|                     MCUTrackPan
```

and in the mst

```
Widget Rotary1
	Encoder b0 10 7f [ > 01-0f < 41-4f ]
	FB_Encoder b0 10 7f
	Toggle 90 20 7f
WidgetEnd

```

You can still define and use the RotaryPush widget for other tasks -- e.g. FXMenu.

```
Widget RotaryPush1
	Press 90 20 7f
WidgetEnd
```


