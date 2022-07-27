If you read the [[Defining Control Surface Capabilities]] page, you'd know that we define Widgets in our .mst/.ost files to represent the capabilities of our surfaces. I sometimes think of these Widgets (or at least the Message Generator part) as triggers I can use to [[cause certain actions or behaviours|Defining Control Surface Behaviour]] to occur.

If we run with that analogy, then the Widgets on our surfaces are not the only things we may want to use to trigger behaviours. Sometimes we may want to trigger them when certain things occur in Reaper itself, e.g. when the selected track changes. 

These are called Virtual Widgets. We don't have to define them in our MST files, as they are not coming from a control surface. We just need to use them in our zone files like any other Widget.

The Virtual Widgets currently defined are:
* [[OnInitialization|Virtual Widgets#OnInitialization]] -- fires when CSI initializes 
* [[OnTrackSelection|Virtual Widgets#OnTrackSelection]] - fires when a track is selected in Reaper. 
* [[OnPageEnter|Virtual Widgets#OnPageEnter]] - fires after a new Page has been entered.
* [[OnPageLeave|Virtual Widgets#OnPageLeave]] - fires before the old Page has been exited.

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