CSI allows you to invoking multiple Actions from a single control by simply defining more than one action your zone file. This allows you to effectively create CSI macros actions. The actions will be executed in the order they appear in the zone definition.

For example, the below string of actions will toggle showing folders only, or their children.
```
Zone "Buttons"
        Shift+F4     Reaper 41803     //Track: Select all top level tracks
        Shift+F4     Reaper 41665     //Mixer: Show/hide children of selected tracks
        Shift+F4     Reaper 40297     //Track: Unselect all tracks
ZoneEnd 
```

The next example combines some SWS actions with a GoHome command to close plugin windows whenever you GoHome.
```
Zone "Buttons"
    GlobalView                  GoHome
    GlobalView                  Reaper _S&M_WNCLS3        	// Close all floating FX windows
    GlobalView                  Reaper _S&M_WNCLS4        	// Close all FX chain windows
ZoneEnd 
```

The next example shows how you can run multiple actions in a SelectedTrackFXMenu to open the plugin GUI as you map the plugin.
```
Zone "SelectedTrackFXMenu"
    RotaryPushA|                 GoFXSlot
    RotaryPushA|                 Reaper "_S&M_FLOATFX|"
    RotaryPushA|                 Reaper "_S&M_SELFX|"
ZoneEnd
```
