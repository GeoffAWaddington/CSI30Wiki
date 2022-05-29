If you've read through the [[FX Zones]] page, we now have a ReaEQ.zon file setup, but we need some way to activate it. 

One approach is to have it activate along with other FX Zones for the FX on the currently selected track. To enable this, we need to do two things:

### 1. Make sure you're using the SelectedTrackNavigator inside your [[FX Zone|FX-Zones]]

Remember the second line in our ReaEQ.zon file?:

````
Zone "VST: ReaEQ (Cockos)" ReaEQ 
    FocusedFXNavigator
    RotaryG11 FXParam 0 "Freq-Band 1"
    Fader1 FXParam 1 "Gain-Band 1"
    ...
````

It currently says ``FocusedFXNavigator``, so we'd need to change that to say ``SelectedTrackNavigator`` instead.

### 2. Map the MapSelectedTrackFXToWidgets actions to a widget

To actually activate the FX Zone, we need to bind the MapSelectedTrackFXToWidgets action to a widget. 

For example, this could be a button, as this example shows:

````
Zone "Buttons|" 
        ...
	LowerButton| MapSelectedTrackFXToWidgets
        ...
ZoneEnd
````  

Another alternative would be to use one of the [[Virtual Widgets]], namely, ``OnTrackSelection``, which would mean that whenever a track was selected in Reaper, any FX on that track that had FX Zones defined would be activated (assuming of course they had been defined with the ``SelectedTrackNavigator`` as above)

### Notes:
* Just remember to give yourself a way back to your Home Zone. A widget somewhere mapped to GoZone Home will let you exit the FX Zone when you're done and get back to controlling the rest of Reaper. 