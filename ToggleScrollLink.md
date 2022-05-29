Whenever a track is selected in Reaper, we may want the faders on our control surface to bank to display that selected track as track 1. You can toggle this behaviour off and on using the ToggleScrollLink action.

`   someButton ToggleScrollLink`

Also, it may me that you don't want it on track 1. Maybe you want it as track 4, so you can have a few tracks either side of it also available. No problem, simply specify that as the parameter.  

`   someButton ToggleScrollLink 4`


Note: ToggleScrollLink will take pinning into account, so if you have a track pinned to track one, then this will attempt to place the selected track in track 2 instead. 


