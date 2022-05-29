As an alternative to [[MapSelectedTrackFXToWidgets]] you may want to be a little more selective in which FX Zones get activated and when. In this case, we can activate only the FX Zone for the currently active FX window, using FocusedFXNavigator.

Make sure you're using the FocusedFXNavigator inside your [[FX Zone|FX-Zones]]

Remember the second line in our ReaEQ.zon file?:

````
Zone "VST: ReaEQ (Cockos)" ReaEQ 
    FocusedFXNavigator
    RotaryG11 FXParam 0 "Freq-Band 1"
    Fader1 FXParam 1 "Gain-Band 1"
    ...
````

It currently says ``FocusedFXNavigator``. This is what we want, not ``SelectedTrackNavigator``.

Notes:
* Like [[MapSelectedTrackFXToWidgets]], remember to give yourself a way back to your Home Zone. A widget somewhere mapped to GoZone Home will let you exit the FX Zone when you're done and get back to controlling the rest of Reaper. 