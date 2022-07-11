CSI has Broadcast and Receive actions designed to allow for keeping multiple surfaces in sync with one another. For example: you may want a "GoHome" on one surface to send one or more additional surfaces to their respective Home zones also. Or maybe you want to use an FX Menu on one surface to broadcast GoFXSlot messages and actually map the FX on another surface. You could even set both surfaces to broadcast and receive simultaneously if you wanted to keep say an OSC surface used for displays in sync with zone changes on a MIDI surface you used for hardware controls. 

Lets look at some examples on how to set this up...

## Example 1: Using Broadcast and Receive Actions for GoHome messages
Let's say I want to broadcast any GoHome changes in surface #1 to surface #2, so that when I go "Home" on suface #1, surface #2 follows along.

First, we have to tell one surface that "when CSI initializes, this surface will broadcast GoHome changes." We do that in our home.zon of surface #1 using a combination of the OnInitialization [virtual widget](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/Virtual-Widgets), with the Broadcast Home action. So the home.zon for surface 1 would look something like this:

```
Zone "Home"
OnInitialization ToggleEnableFocusedFXMapping 
OnInitialization Broadcast Home 
     IncludedZones
          "SelectedTrack"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd
```

Then, in surface #2, we'd need to express the message that "when CSI initializes, you should receive any broadcasted GoHome messages." We do that very similarly; using the OnInitialization virtual widget but this time we're going to use the Receive Home CSI action. So surface #2 would have a home.zon that might look like this:

```
Zone "Home"
    OnInitialization Receive Home
    IncludedZones
          "Channel"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
    IncludedZonesEnd
ZoneEnd  
```

## Example 2: Using Broadcast and Receive Actions to Keep Two Surfaces in Sync
What if you want two-way communication between two surfaces to make sure they stay in sync? Can you use both Broadcast and Receive actions in the same surface? Yes you can!

I use a TouchOSC setup running on my iPad to mirror my MIDI Fighter Twister hardware and use the same set of zone files on both devices. So in this use case, I want to make sure that regardless of what surface I'm using for controls at the time, that any zone changes or mapping of the FXMenu stay in sync across both devices. So if I map an FX using the GoFXSlot action using the TouchOSC device or the MIDI Fighter Twister, that mapping action gets broadcast and received regardless of which device initiated the action.

Another key point here is that you only need one "OnInitialization Broadcast" row and one "OnInitialization Receive" row per surface with all of the broadcast/receive types listed out after. This is what the home.zon file for this setup would look like:

```
Zone "Home"
OnInitialization Broadcast Home SelectedTrackFXMenu SelectedTrackSend SelectedTrackReceive
OnInitialization Receive   Home SelectedTrackFXMenu SelectedTrackSend SelectedTrackReceive
     IncludedZones
          "SelectedTrack"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd
```


## Broadcast and Receive Action List
The list of broadcast and receive actions is as follows:

```
* Broadcast
* Receive
* Home
* SelectedTrackFXMenu
* TrackFXMenu
* GoFXSlot
* SelectedTrackSend
* TrackSend
* SelectedTrackReceive
* TrackReceive
```