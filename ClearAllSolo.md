ClearAllSolo is a CSI action designed to clear all solo'd tracks and also work with the global MCU 'Solo' widget to provide a visual indication that one or more tracks in your project may be in a solo'd state. 

Example: you have an MCU surface, and Reaper tracks 1-8 are currently mapped to the surface's 8 faders, but you've got track 32 solo'd. When assigned to a widget capable of displaying two-state feedback, your surface will display a light indicating there is a solo'd track anywhere in your project. Pressing the button assigned to this action, will then clear any and all solo'd tracks.

````
Zone "Buttons|"
        Solo            ClearAllSolo
ZoneEnd
````