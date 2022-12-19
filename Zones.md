
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


## One Zone Per File 
CSI requires that a single zone exist in each .zon file. For example: a typical MCU-style zone folder would be made up of multiple .zon files such as...
```
Home.zon
Track.zon
TrackSend.zon
TrackReceive.zon
TrackFXMenu.zon
SelectedTrackSend.zon
SelectedTrackReceive.zon
SelectedTrackFXMenu.zon
Buttons.zon
MasterTrack.zon
Marker.zon
FocusedFXParam.zon
``` 

This makes it easier to borrow other people's zones and add them to your setup. Example: if you see a SelectedTrackSendSlot.zon file that works for the X-Touch, you can just copy and paste that into your surface folder, and use that with a Mackie Universal as long as the widget names are the same.

**Tip:** within each **\CSI\Zones\[Surface]** folder, it's probably a best practice to create an "FX Zones" sub-folder if you plan on mapping FX. This way, your surface zone files can all live together in the root of the folder, and all of your fx zones can exist in the **\CSI\Zones\[Surface]\FX Zones** folder, away from the surface files. Not a requirement, but will help keep things tidy.

## Naming Zones / Fixed Zone Types
Starting in CSI v2.0, Zones and AssociatedZones must have a fixed name. These zone types are:

* **Home**- This zone is required and is the 'starting state' for CSI
* **Buttons**- This zone is generally used for assigning buttons to CSI and Reaper actions
* **Track**- Used when you want to control multiple channels across multiple widgets (e.g. 8 faders assigned to 8 channels)
* **SelectedTrack**- Used for controlling the selected track in Reaper (commonly used for 1 fader surfaces)
* **MasterTrack**- This is for assigning the master track fader to your surface
* **SelectedTrackFXMenu**- Used for activating FX.zon files - shows the FX slots of the selected channel
* **SelectedTrackSend**- Used for controlling the various sends on the selected channel
* **SelectedTrackReceive**- Used for controlling the various Receives (if any) on the selected channel
* **TrackFXMenu**- Used for activating FX.zon files - shows the same FX Slot across multiple channels
* **TrackSend**- Used for controlling the same Send slot across multiple channels
* **TrackReceive**- Used for controlling the same Receive slot across multiple channels  


## Activating Zones
Depending on the type of zone you want to activate, there are few methods to accomplish this task. If you're looking to activate one of the zone types with a fixed name, there are dedicated CSI actions for each type...

```
Zone you want to activiate...   ActionName...
Home                            GoHome
SelectedTrackFXMenu             GoSelectedTrackFXMenu
SelectedTrackSend               GoSelectedTrackSend
SelectedTrackReceive            GoSelectedTrackReceive
TrackFXMenu                     GoTrackFXmenu
TrackReceive                    GoTrackReceive
TrackSend                       GoTrackSend
```

Here's what that looks like in a Buttons.zon
````
    MidiTracks                  GoSelectedTrackSend
    Inputs                      GoSelectedTrackReceive
    AudioTracks                 GoSelectedTrackFXMenu
    AudioInstrument             GoTrackSend
    Aux                         GoTrackReceive
    Busses                      GoTrackFXMenu
````

## Home Zone
Every surface in CSI requires a Home zone. The types of zones defined in "IncludedZones" will dictate the starting (or "home") state of the surface. Additionally, CSI version 2 introduced the concept of AssociatedZones. These are zones like Sends, Receives, and FX Menus that are not activated as part of the home.zon, but will be called from this zone.

Below is an example of a typical MCU-style home.zon. The "Track" zone will use the displays and widgets when the Home zone is active, but if you want to call up an FX menu, Sends, or Receives to then takeover over some of the widgets, they need to be listed as AssociatedZones as shown below. When configured like this, the AssociatedZones function as "radio-button" style zones, where only one can be active at a given time (example: SelectedTrackSends or SelectedTrackReceives - not both simultaneously).
```
Zone Home
    IncludedZones
        "Buttons"
        "Track"
        "MasterTrack"
    IncludedZonesEnd
    AssociatedZones
       "SelectedTrackFXMenu"
       "SelectedTrackSend"
       "SelectedTrackReceive"
       "TrackFXMenu"
       "TrackSend"
       "TrackReceive"
    AssociatedZonesEnd
ZoneEnd
```

On the other hand, here's an example of a home.zon where the Track zone, SelectedTrackFXMenu, and SelectedTrackSend zones are all activate in the home.zon itself. For this particular device/use-case, RowA of the surface covers the Track zone, RowB covers the SelectedTrackFXMenu, and RowC covers SelectedTrackSend and I wanted all 3 active simultaneously which is why we have all 3 listed as IncludedZones in the Home.zon versus AssociatedZones in the prior example.
```
Zone "Home"
    IncludedZones
        "Buttons"
        "Track"
        "SelectedTrackFXMenu"
        "SelectedTrackSend"
    IncludedZonesEnd
ZoneEnd
```

The Home.zon would also include any [[Broadcast and Receive|Broadcast-and-Receive]] messages and is also a great place to define actions assigned to [[Virtual Widgets]].

## FX Zones
FX zone file names can be whatever you'd like them to, but the FX zone name in the .zon file itself must match the plugin name in Reaper exactly. See [FX Zones](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/FX-Zones) for more information on how to create an FX.zon.

## SubZones
If you need to create a custom zone, like a "Zoom" zone, or a "FocusedFXParam" zone, these must be created as SubZones and called by the action GoSubZone. Example:

```
Zone "Buttons"
    Zoom        GoSubZone "Zoom"
ZoneEnd
```

```
Zone "Zoom"
     Up                      Reaper 40111     // Zoom in vertical
     Down                    Reaper 40112     // Zoom out vertical
     Left                    Reaper 1011      // Zoom out horizontal
     Right                   Reaper 1012      // Zoom in horizontal
     
     Shift+Up                Reaper 40113     // View: Toggle track zoom to maximum height
     Shift+Down              Reaper 40110     // View: Toggle track zoom to minimum height
     Shift+Left              Reaper 40295     // View: Zoom out project
     Shift+Right             Reaper 41190     // View: Set horizontal zoom to default project setting

     Zoom LeaveSubZone
ZoneEnd
```

SubZones are also commonly used in FX, for example, where a mastering suite plugin may have lots of different sections and more automation than you have on your surface. In those instances, you could create a "compressor" SubZone, and a "limiter" SubZone, etc.

## A Slightly More Useful Example

As great as it is to get that first action happening when you press a Surface button, let’s look at a slightly more interesting example. A common surface setup might include a Buttons.zon that has various CSI and Reaper actions that can be called up from the surface. Some of these buttons may even call other zones. Here's an excerpt of a Buttons.zon:

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
