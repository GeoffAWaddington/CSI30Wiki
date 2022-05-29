There are several Actions related to selecting a track in Reaper. They all need to be used in a Zone that has a [[TrackNavigator|Navigators]], so they know which track to be working on :

* TrackSelect : Selects the track. If there are other tracks already selected, adding it to the set of any other currently selected tracks. 
* TrackUniqueSelect : Selects the track, and any other selected tracks will be deselected.
* TrackRangeSelect : Select the start and end of a range of tracks and have them all selected. 

Here's an example of how they can be used:
```
Zone "Channel"
    TrackNavigator
    Select|                     TrackUniqueSelect
    Shift+Select|               TrackRangeSelect
    Control+Select|             TrackSelect
ZoneEnd
```