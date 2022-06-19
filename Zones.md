
Zones are where we do two things:

* where we map the widgets defined in our mst files to the actions we want to perform when they are invoked (pressed, rotated, etc)
* where we group multiple action/widget mappings together into related groups. 

## A Simple Example

The widget/action mapping part is easy. Let's say we have a widget called PlayButton, and we want it to perform the Play action when pressed, we would simply use the following syntax in our zon file. 

````
PlayButton Play
````

There's obviously more options than just play. Have a look at the [[Action Reference]] page for more examples. 

However, we can't just have this sitting by itself. We need to put it inside a zone definition, usually along with other widgets performing related functions (say, the Stop, Record, FF, Rewind buttons, etc). At a minimum, we’d need something like this:

````
Zone Home
        PlayButton Play
ZoneEnd
````

Provided our [[CSI.INI]] and Widget definitions are setup correctly, this should allow us to control the Play button from our Surface.


## One Zone Per File (CSI 1.1 and Up)
CSI version 1.1 introduced a major change in that now you can only have one zone per file. In the past, a surface.zon file was usually comprised of multiple zones like: Home, Track, Button, Mastertrack, Jogwheel, SelectedTrackSends. Starting with CSI 1.1, these now need to be separate files. A similar CSI 1.1 surface would now be comprised of multiple files like:

```
Home.zon
Track.zon
Button.zon
MasterTrack.zon
JogWheel.zon
SelectedTrackSend.zon
``` 

This change immensely improves performance when you have a large number of FX.zon's but also has the added benefit of making it easier to borrow other people's zones and more easily add them to your setup. Example: if you see a SelectedTrackSendSlot.zon file that works, you can just copy and paste that into your surface folder, and just add the appropriate mapping action to your home.zon.

**Tip:** within each **\CSI\Zones\[Surface]** folder, it's probably a best practice to create an "FX Zones" sub-folder if you plan on mapping FX. This way, your surface zone files can all live together in the root of the folder, and all of your fx zones can exist in the **\CSI\Zones\[Surface]\FX Zones** folder, away from the surface files. Not a requirement, but will help keep things tidy.

## Naming Zones
Starting in CSI v2.0, Zones and AssociatedZones must have a fixed name. These zone types are:

```
Home
Buttons
Track
SelectedTrack
MasterTrack
SelectedTrackFXMenu
SelectedTrackSend
SelectedTrackReceive
TrackFXMenu
TrackReceive
TrackSend
```

If you need to create a custom zone, like a "Zoom" zone, or a "FocusedFXParam" zone, these must be created as SubZones and called by the action GoSubZone. Example:

```
Zone "Buttons"
    Zoom        GoSubZone "Zoom"
ZoneEnd
```

```
SubZone "Zoom"
     Up                      Reaper "40111"     // Zoom in vertical
     Down                    Reaper "40112"     // Zoom out vertical
     Left                    Reaper "1011"      // Zoom out horizontal
     Right                   Reaper "1012"      // Zoom in horizontal
     
     Shift+Up                Reaper "40113"     // View: Toggle track zoom to maximum height
     Shift+Down              Reaper "40110"     // View: Toggle track zoom to minimum height
     Shift+Left              Reaper "40295"     // View: Zoom out project
     Shift+Right             Reaper "41190"     // View: Set horizontal zoom to default project setting

     Zoom LeaveSubZone
SubZoneEnd
```

FX zone file names can be whatever you'd like them to, but the FX zone name in the .zon file itself must match the plugin name in Reaper exactly. See [FX Zones](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/FX-Zones) for more information on how to create an FX.zon.


## A Slightly More Useful Example

As great as it is to get that first action happening when you press a Surface button, let’s look at a slightly more interesting example. A common surface setup might include a Buttons.zon that has various CSI and Reaper actions that can be called up from the surface. Some of these buttons may even call other zones. Here's an excerpt of a buttons.zon:

````
Zone "Buttons"
    Track                       Reaper 1156	// Toggle item grouping override
    Pan                         Reaper 1155	// Cycle ripple editing mode
    EQ                          Reaper 40070	// Move envelope points with media items
    Send                        Reaper 40145 	// Toggle Grid Lines
    Hold+Send                   Reaper 40071	// Show snap/grid settings
    Plugin                      Reaper 1157	// Toggle snapping
    Hold+Plugin			Reaper 40071	// Show snap/grid settings
    Instrument                  Reaper 1135	// Toggle locking
    Hold+Instrument		Reaper 40277	// Show lock settings
    BankLeft                    TrackBank -8
    BankRight                   TrackBank 8
    ChannelLeft                 TrackBank -1
    ChannelRight                TrackBank 1
    Flip                        Flip
    GlobalView                  GoHome
    GlobalView                  Reaper _S&M_WNCLS3        	// Close all floating FX windows
    GlobalView                  Reaper _S&M_WNCLS4        	// Close all FX chain windows
    GlobalView                  Reaper _S&M_TOGLFXCHAIN   	// Toggle FX Chain for selected tracks     
    nameValue                   NoAction   
    smpteBeats                  CycleTimeDisplayModes
    MidiTracks                  GoSelectedTrackSend
    Inputs                      GoSelectedTrackReceive
    AudioTracks                 GoSelectedTrackFXMenu
    AudioInstrument             GoTrackSend
    Aux                         GoTrackReceive
    Busses                      GoTrackFXMenu
    Outputs                     ToggleEnableFocusedFXMapping 
    User                        ToggleEnableFocusedFXParamMapping
ZoneEnd
````

As another example, I might define a zone called Track which collects all my track faders and related widgets into a single group, again because for me they are all related to controlling my tracks. 
````
Zone "Track"
    DisplayUpper|               TrackNameDisplay
    Fader|Touch+DisplayLower|   TrackVolumeDisplay
    DisplayLower|               MCUTrackPanDisplay
    VUMeter|                    TrackOutputMeterMaxPeakLR
    Fader|                      TrackVolume 
    Flip+Fader|                 TrackPan 
    Rotary|                     MCUTrackPan
    RotaryPush|                 ToggleMCUTrackPanWidth
    RecordArm|                  TrackRecordArm
    Solo|                       TrackSolo
    Mute|                       TrackMute
    Select|                     TrackUniqueSelect
    Shift+Select|               TrackRangeSelect
    Control+Select|             TrackSelect
ZoneEnd
````

Note the | character after the name of the widgets. These will get replaced with the actual number derived from the number of channels you entered during setup. It is simply a shorthand way of reducing typing. So for example if you specified 8 channels this line:

Fader|  TrackVolume

will cause this to be generated automatically by CSI...

Fader1  TrackVolume

Fader2  TrackVolume

Fader3  TrackVolume

Fader4  TrackVolume

Fader5  TrackVolume

Fader6  TrackVolume

Fader7  TrackVolume

Fader8  TrackVolume


## Changing Zones
We can also change which zones are active at the moment using an Action called GoZone:

`SomeButton GoZone "AnotherZone"`

That is telling CSI to "overlay" that zone over the currently active zones. Any widgets used in AnotherZone that were previously mapped in the currently active zones  will now trigger the actions defined in the anotherZone. Any that aren't used in AnotherZone will keep performing the actions they were before we triggered [[GoZone|Zone Actions]]. 

This results in a very flexible system where can temporarily change certain control's behavior in response to whatever widgets and/or [[Virtual Widgets]] we like.

**Note:**  If a zone name contains a space, like "Selected Channel Buttons" for example, then any GoZone actions must have the zone name in quotes. As a best practice, if you always include the zone name in quotes (even when not technically required), you'll never have to worry. 

## Adding Comments to Zones
When triggering custom actions or using actions whose meanings may otherwise not be obvious, it's a good idea to add comments to your in your .zon files. CSI allows for two types of comments:

1. / Lines that begin with a forward slash are completely ignored by CSI and are good for commenting sections of code. **Hint:** these also work in .mst files.
2. // Trailing comments are preceeded by two forward slashes and can be used after an action in a .zon file

Here is an example from a .zon file that uses both types of comments.

````
Zone "Buttons"

/ Using the GlobalView button to activate the Home zone and then run the actions to close the floating FX windows.
    GlobalView                  GoHome
    GlobalView                  Reaper _S&M_WNCLS3        	// Close all floating FX windows
    GlobalView                  Reaper _S&M_WNCLS4        	// Close all FX chain windows
    GlobalView                  Reaper _S&M_TOGLFXCHAIN   	// Toggle FX Chain for selected tracks     
````
