If you read the [[Defining Control Surface Capabilities]] page, you'd know that we define Widgets in our .mst/.ost files to represent the capabilities of our surfaces. I sometimes think of these Widgets (or at least the Message Generator part) as triggers I can use to [[cause certain actions or behaviours|Defining-Control-Surface-Behavior]] to occur.

If we run with that analogy, then the Widgets on our surfaces are not the only things we may want to use to trigger behaviours. Sometimes we may want to trigger them when certain things occur in Reaper itself, e.g. when the selected track changes. 

These are called Virtual Widgets. We don't have to define them in our MST files, as they are not coming from a control surface. We just need to use them in our zone files like any other Widget.

The Virtual Widgets currently defined are:
* [[OnInitialization|Virtual Widgets#OnInitialization]] -- fires when CSI initializes 
* [[OnTrackSelection|Virtual Widgets#OnTrackSelection]] - fires when a track is selected in Reaper
* [[OnPageEnter|Virtual Widgets#OnPageEnter]] - fires after a new Page has been entered
* [[OnPageLeave|Virtual Widgets#OnPageLeave]] - fires before the old Page has been exited
* [[OnPlayStart|Virtual Widgets#OnPlayStart]] - fires when playback starts
* [[OnPlayStop|Virtual Widgets#OnPlayStop]] - fires when playback stops 
* [[OnRecordStart|Virtual Widgets#OnPlayStart]] - fires when recording starts
* [[OnRecordStop|Virtual Widgets#OnPlayStop]] - fires when recording stops 
* [[OnZoneActivation|Virtual Widgets#OnZoneActivation]] - fires when the zone it appears in has been activated
* [[OnZoneDeactivation|Virtual Widgets#OnZoneDeactivation]] - fires when the zone it appears in has been exited

## OnInitialization
The OnInitialization virtual widget belongs in your home.zon file and fires when CSI is initialized. For instance, OnInitialization could be to turn Focused FX mapping (ToggleEnableFocusedFXMapping) to an off state as it is on by default. If you're using an FX Menu, this is probably something you'd want off by default. This would be as simple as adding OnInitialization ToggleEnableFocusedFXMapping to your home zone.
```
Zone "Home"
OnInitialization ToggleEnableFocusedFXMapping 
     IncludedZones
          "SelectedTrack"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd
```

## OnTrackSelection 
OnTrackSelection is another virtual widget and will fire every time you select a new track in Reaper. Example: maybe you want selecting a new track to return to your home.zon and close the GUI of any open plugin windows. You can automate all of that by doing something like this.
```
Zone "Home"
OnTrackSelection          GoHome
OnTrackSelection          Reaper _S&M_WNCLS4                      // Closes All(!) FX chain windows
OnTrackSelection          Reaper _S&M_WNCLS3                      // Closes All(!) Floating FX windows
     IncludedZones
          "SelectedTrack"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd
```

## OnPageEnter
OnPageEnter will fire an action when a new [[Page|Pages]] is activated in CSI. Note: if you wanted this same action to occur when Reaper/CSI is initialized, then you'd need to combine that with an OnInitialization virtual widget and action.

For instance, in the Home.zon on one of my surfaces in my "HomePage", I may have this to set a particular screenset on startup and page enter.
```
Zone "Home"
OnInitialization Reaper 40454     // Screenset: Load window set #01        
OnPageEnter      Reaper 40454     // Screenset: Load window set #01          
     IncludedZones
          "SelectedTrack"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd  
```

And in the Home.zon on that same surface I may have this on a hypothetical "Mix" page to load Screenset #2 whenever I enter that page, but then it will also automatically run the SWS action to then toggle the narrow mixer mode..
```
Zone "Home"    
OnPageEnter      Reaper 40455                  // Screenset: Load window set #02
OnPageEnter      Reaper _S&M_CYCLACTION_17     // Narrow Mixer Toggle
     IncludedZones
          "SelectedTrack"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd  
```

## OnPageLeave
OnPageEnter will fire an action just prior to exiting the current [[Page|Pages]] in CSI. Expanding on the above example, entering the "Mix" Page toggles the narrow mixer mode, and exiting the "Mix" Page automatically toggles it back. 

```
Zone "Home"    
OnPageEnter      Reaper 40455                  // Screenset: Load window set #02
OnPageEnter      Reaper _S&M_CYCLACTION_17     // Narrow Mixer Toggle
OnPageLeave      Reaper _S&M_CYCLACTION_17     // Narrow Mixer Toggle
     IncludedZones
          "SelectedTrack"
          "Buttons"
          "SelectedTrackFXMenu"
          "SelectedTrackSend"
          "SelectedTrackReceive"
     IncludedZonesEnd
ZoneEnd  
```

## OnPlayStart
You can use OnPlayStart to fire particular CSI or Reaper actions when playback begins. In the below example, it's used in a Home.zon to fire the SendMIDIMessage action to cause a button to start strobing.
```
Zone "Home"
OnPlayStart   SendMIDIMessage "B5 0E 04"     // Makes button B7 strobe on play start
...
ZoneEnd
```

## OnPlayStop
This is the inverse action fo OnPlayStart and fires when playback stops. Building upon the example above, I'm sending another MIDI message when playback stops to restore the non-blinking button state.
```
Zone "Home"
OnPlayStart   SendMIDIMessage "B5 0E 04"     // Makes button B7 strobe on play start
OnPlayStop    SendMIDIMessage "B5 0E 00"     // Makes button B7 stop strobing on play start
...
ZoneEnd
```

## OnRecordStart
This action will fire when recording starts in Reaper. In the below example, I'm using it to make button B8 strobe when Reaper is recording.
```
Zone "Home"
OnRecordStart SendMIDIMessage "B5 0F 04"     // Makes button B8 strobe on record start
````

## OnRecordStop
This action will fire when recording stops in Reaper. In the below example, I'm using it to make button B8 stop strobing when Reaper comes out of recording mode.
```
Zone "Home"
OnRecordStart SendMIDIMessage "B5 0F 04"     // Makes button B8 strobe on record start
OnRecordStop  SendMIDIMessage "B5 0F 00"     // Makes button B8 stop strobing on record stop
````

## OnZoneActivation
OnZoneActivation fires whenever the zone this command appears in becomes activated. One use-case for this action is setting the widget state for one or more controls on your surface as shown in the examples below. The first two examples set all of the displays on an X-Touch Universal to a cyan color when the SelectedTrackSend is activated, and yellow when the SelectedTrackFXMenu is activated for additional visual cues.
```
Zone "SelectedTrackSend"
	OnZoneActivation	    SetXTouchDisplayColors Cyan
 ...
ZoneEnd
```

```
Zone "SelectedTrackFXMenu"
	OnZoneActivation	SetXTouchDisplayColors Yellow
...
ZoneEnd
```

The next example is useful for Reaper users using the [OSARA](https://osara.reaperaccessibility.com/) extension as a screen reader. The intent here is to read the name of the plugin aloud whenever an FX.zon becomes activated.
``` 
Zone "VST: UAD Fairchild 660 (Universal Audio, Inc.)" "Fair660"
	OnZoneActivation	Speak "UAD Fairchild 660 Compressor"

	DisplayUpper1		FixedTextDisplay "HdRoom"
 	DisplayLower1		FXParamValueDisplay 9
	Rotary1			FXParam 9 [ 0.0 0.17 0.33 0.50 0.67 0.83 1.0 ]
   ...
ZoneEnd
```

## OnZoneDeActivation
OnZoneDeactivation fires whenever the zone this command appears in becomes deactivated (i.e. exited). Expanding on our first two examples, we can then restore the display colors on the X-Touch Universal when the SelectedTrackSend or SelectedTrackFXMenu are exited.
```
Zone "SelectedTrackSend"
	OnZoneActivation	    SetXTouchDisplayColors Cyan
	OnZoneDeactivation	    RestoreXTouchDisplayColors
 ...
ZoneEnd
```

```
Zone "SelectedTrackFXMenu"
	OnZoneActivation	SetXTouchDisplayColors Yellow
	OnZoneDeactivation	RestoreXTouchDisplayColors
...
ZoneEnd
```