FocusedFXParam is a CSI action that allows you to assign a control on your surface to the last touched plugin parameter. This can be a very fast way to assign plugin parameters to your surface without having to create an fx.zon in advance. This is very useful for quickly writing automation or tweaking a plugin parameter.

For instance, let's say you already have Fader1 on your surface mapped to track volume of the Selected track, and you want to map Shift+Fader1 the last-touched FX parameter. Your .zon file might look like this...

```` 
Zone "Channel"
        SelectedTrackNavigator
        Fader1                  TrackVolume
	Shift+Fader1 		FocusedFXParam
ZoneEnd
````

Now, when you enable the Shift modifier, Fader1 will control the last touched plugin parameter. If you want to change the parameter you're controlling, just use your mouse and move the next plugin parameter. Your surface will update to control the new parameter as long as you're still in Shift mode (or when you next hold down the shift modifier). **Tip:** a quick tap of Shift latches the shift modifier - very handy in this example.

Taking this a step furter, we can also use [[Zone changing|Zones#changing-zones]] to alternate between Fader 1 as controlling track volume in the Home zone (Shift+F1) or the FocusedFXParam in that zone (Shift+F2). Note: I'm using the Shift modifer plus the F1 and F2 widgets in the below example, but you could use any combination of modifiers and/or widgets. You'll also notice that a  [[Navigator|Navigators]]  is not required for FocusedFXParam to work.
```` 
Zone "GlobalButtons|"
	Shift+F1			GoZone Home
	Shift+F2			GoZone ZoneThatMapsFocusedFXParamsTowidgets
ZonEnd

Zone "Channel|"
	TrackNavigator
	Fader|  			TrackVolume
ZoneEnd

Zone "ZoneThatMapsFocusedFXParamsTowidgets"
         Fader1 FocusedFXParam
ZoneEnd
```` 

## FocusedFXParamNameDisplay and FocusedFXParamValueDisplay
Now, building off the GoZone example above, let's say our surface has displays and we want the upper display to show the Parameter Name and lower display to show the parameter value whenever the FocusedFXParam zone is active. The FocusedFXParamNameDisplay and FocusedFXParamValueDisplay actions are designed to do just that. 

```` 
Zone "ZoneThatMapsFocusedFXParamsTowidgets"
        Fader1 FocusedFXParam
	DisplayUpper1 FocusedFXParamNameDisplay
	DisplayLower1 FocusedFXParamValueDisplay
ZoneEnd
```` 