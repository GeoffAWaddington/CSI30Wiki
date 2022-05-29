CSI version 1.1 introduced a series of Broadcast and Receive actions designed to allow for keeping multiple surfaces in sync with one another. For example: if you want to use an FX Menu on one surface, to broadcast the GoFXSlot message to actually map the FX on another surface, that's now possible. You could even set both surfaces to broadcast and receive simultaneously if you wanted to keep say an OSC surface used for displays in sync with zone changes on a MIDI surface you used for hardware controls. And if you wanted to use a button on a third surface to take all surfaces back to their respective "Home" zones, you can!

## Example 1: Using Broadcast and Receive Actions for GoZone messages
Rather than explain each broadcast and receive action individually, let's get into some real-world examples. Let's say I want to broadcast any GoZone changes in surface #1 to surface #2, so that when I go "Home" on suface #1, surface #2 follows along.

First, we have to tell one surface that "when CSI initializes, this surface will broadcast GoZone changes." We do that in our home.zon of surface #1 using a combination of the OnInitialization [virtual widget](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/Virtual-Widgets), with the SetBroadcastGoZone action. So the home.zon for surface 1 would look something like this:

```
Zone "Home"
    OnInitialization SetBroadcastGoZone 
    IncludedZones
          "Channel"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
    IncludedZonesEnd
ZoneEnd
```

Then, in surface #2, we'd need to express the message that "when CSI initializes, you should receive any broadcasted GoZone messages." We do that very similarly; using the OnInitialization virtual widget but this time we're going to use the SetReceiveGoZone CSI action. So surface #2 would have a home.zon that might look like this:

```
Zone "Home"
    OnInitialization SetReceiveGoZone 
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
What if you want two-way communication between two surfaces to make sure they stay in sync? Can you use both SetBroadcast and SetReceive actions in the same surface? Yes you can!

I use a TouchOSC setup running on my iPad to mirror my MIDI Fighter Twister hardware, and use the same set of zone files on both devices. So in this use case, I want to make sure that regardless of what surface I'm using for controls at the time, that any zone changes or mapping of the FXMenu stay in sync across both devices. So if I map an FX using the GoFXSlot action using the TouchOSC device or the MIDI Fighter Twister, that mapping action gets broadcast and received regardless of which device initiated the action.

This is how the home.zon file for this setup looks:

```
Zone "Home"
    OnInitialization SetBroadcastGoZone 
    OnInitialization SetReceiveGoZone
    OnInitialization SetReceiveMapSelectedTrackFXToMenu
    OnTrackSelection UnmapSelectedTrackFXFromMenu 
    OnInitialization SetBroadcastGoFXSlot 
    OnInitialization SetReceiveGoFXSlot
     IncludedZones
          "SelectedChannel"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd
```


## Broadcast and Receive Action List
The full list of broadcast and receive actions is as follows:

* SetBroadcastGoZone 
* SetReceiveGoZone
* SetBroadcastGoFXSlot
* SetReceiveGoFXSlot
* SetBroadcastMapSelectedTrackSendsToWidgets
* SetReceiveMapSelectedTrackSendsToWidgets
* SetBroadcastMapSelectedTrackReceivesToWidgets
* SetReceiveMapSelectedTrackReceivesToWidgets
* SetBroadcastMapSelectedTrackFXToWidgets
* SetReceiveMapSelectedTrackFXToWidgets
* SetBroadcastMapSelectedTrackFXToMenu
* SetReceiveMapSelectedTrackFXToMenu
* SetBroadcastMapTrackSendsSlotToWidgets
* SetReceiveMapTrackSendsSlotToWidgets
* SetBroadcastMapTrackReceivesSlotToWidgets
* SetReceiveMapTrackReceivesSlotToWidgets
* SetBroadcastMapTrackFXMenusSlotToWidgets
* SetReceiveMapTrackFXMenusSlotToWidgets

Now that you know how the SetBroadcast and SetReceive messages work, you just need to know when to use each. For details on that you need to understand how the GoZone and mapping actions work, so see the page for the corresponding CSI action. Example: for SetBroadcastMapSelectedTrackSendsToWidgets and SetReceiveMapSelectedTrackSendsToWidgets, see the page on [MapSelectedTrackSendsToWidgets](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/Send-Zones#send-mapping-and-unmapping).