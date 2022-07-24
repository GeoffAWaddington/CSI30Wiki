* [[SaveProject|Project Actions#SaveProject]]
* [[Undo|Project Actions#Undo]]
* [[Redo|Project Actions#Redo]]

## SaveProject
The SaveProject action will save your current your current project in Reaper. The below example would suit an MCU type surface with a dedicated Save button, but the CSI action can be assigned to any button on any surface.
```
Zone "Buttons"
    Save     SaveProject
ZoneEnd
```

## Undo
The Undo action will undo the last action in Reaper (as long as that action is captured in Reaper's Undo History). The below example would suit an MCU type surface with a dedicated Undo button, but the CSI action can be assigned to any button on any surface.
```
Zone "Buttons"
    Undo     Undo
ZoneEnd
```

## Redo
The Redo action will redo the last action in Reaper (as long as an action captured by Reaper's Undo History was recently undone). Here it's being used with a Shift modifier combined with the Undo button.
```
Zone "Buttons"
    Shift+Undo     Undo
ZoneEnd
```