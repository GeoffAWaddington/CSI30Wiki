As mentioned on the [[Installation|Installation and Setup]] page, the configuration of your CSI installation is ultimately stored in a file called csi.ini. This normally gets created when you setup CSI for the first time and shouldn't require direct user editing, however, if you want to understand how the csi.ini works, or need to troubleshoot, read on!

Here is what a typical CSI.ini might look like:
```
Version 2.0

MidiSurface "X-Touch" 12 11 
OSCSurface "iPad Pro" 8000 9000 10.0.0.146 

Page "HomePage" 
"X-Touch" 8 0 "X-Touch.mst" "X-Touch"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterMenu"
```

The CSI version number is included in the first row. If missing, CSI will throw up an error when Reaper starts.

Next, you see...
```
MidiSurface "X-Touch" 12 11 
```

This tells CSI that the user has configured a MIDI surface, named this device "X-Touch", and that device uses MIDI port 12 for incoming messages and MIDI port 11 for outbound messages.

There's also an OSC device in this setup.
```
OSCSurface "iPad Pro" 8000 9000 10.0.0.146 
```

This is telling CSI that there is an OSC surface that the user has named "iPad Pro", and that receives on port 8000, transmits to port 9000, and that iPad Pro has an IP Address of 10.0.0.146.

Next, each [[Page|Pages]] in CSI is defined, with the surfaces, .mst file, and zone folder that will be used for each page.
```
Page "HomePage" 
"X-Touch" 8 0 "X-Touch.mst" "X-Touch"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterMenu"
```

```
Page "HomePage"
```
...is the name of the page.

```
"X-Touch" 8 0 "X-Touch.mst" "X-Touch"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterMenu"
```

This says the X-Touch surface gets included in the HomePage, has 8 channels, a channel offset of 0, and uses the X-Touch.mst and "X-Touch" zone folder. The iPad Pro definition follows the same format.

If you had multiple pages defined, they would follow this same format as shown here...
```
Version 2.0

MidiSurface "XTouchOne" 7 9
MidiSurface "MFTwister" 6 8 
OSCSurface "iPad Pro" 8003 9003 10.0.0.146 

Page "HomePage" 
"XTouchOne" 1 0 "X-Touch_One.mst" "X-Touch_One_SelectedTrack"
"MFTwister" 8 0 "MIDIFighterTwisterEncoder.mst" "FXTwisterMenu"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterMenu"

Page "FocusedFXPage" 
"XTouchOne" 1 0 "X-Touch_One.mst" "X-Touch_One_SelectedTrack"
"MFTwister" 8 0 "MIDIFighterTwisterEncoder.mst" "FXTwisterFocusedFX"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterFocusedFX"
```

## Page Configuration Options
There are some additional options one can add in the csi.ini to modify functionality of CSI on a per-[[Page|Pages]] level. These are entirely optional, and in no means required but exist solely to override the default behavior. These are:

* **FollowTCP** - By default CSI follows the track visibility of Reaper's MCP view. Use Follow TCP to override the default functionality.
* **UseScrollLink** - This turns on scroll linking between the surface and Reaper (Reaper follows CSI). The default is off.
* **NoSynchPages** - With this disabled, each page will have independent banking. The default is on.

From a syntax perspective, these belong immediately following the Page name for any Pages where you are looking to override the default behavior. 

```
Version 2.0

MidiSurface "XTouchOne" 7 9
MidiSurface "MFTwister" 6 8 
OSCSurface "iPad Pro" 8003 9003 10.0.0.146 

Page "HomePage" FollowTCP UseScrollLink NoSynchPages
"XTouchOne" 1 0 "X-Touch_One.mst" "X-Touch_One_SelectedTrack"
"MFTwister" 8 0 "MIDIFighterTwisterEncoder.mst" "FXTwisterMenu"
"iPad Pro" 8 0 "FXTwister.ost" "FXTwisterMenu"
```