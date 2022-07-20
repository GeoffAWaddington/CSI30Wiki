* [[TrackBank|Navigation Actions#TrackBank]]
* [[SelectedTrackBank|Navigation Actions#SelectedTrackBank]]
* [[TrackSendBank]]
* [[TrackReceiveBank]]
* [[TrackFXMenuBank]]
* [[SelectedTrackSendBank]]
* [[SelectedTrackReceiveBank]]
* [[SelectedTrackFXMenuBank]]
* [[ToggleSynchPageBanking]]
* [[ToggleScrollLink]]
* [[ForceScrollLink]]
* [[NextPage|Pages#paging-actions]]
* [[GoPage|Pages#paging-actions]]
* [[GoHome]]
* [[GoSubZone|FX-Sub-Zones]]
* [[LeaveSubZone]]
* [[PageNameDisplay|Pages#pagenamedisplay]]
* [[GoTrackSend]]
* [[GoTrackReceive]]
* [[GoTrackFXMenu]]
* [[GoSelectedTrackSend]]
* [[GoSelectedTrackReceive]]
* [[GoSelectedTrackFXMenu]]
* [[GoSelectedTrackFX]]

## TrackBank
Add this action to your Buttons zone for banking the surface in a Track context. No change is made to the track selection in Reaper. Positive or negative numbers after the action name will dictate how many tracks will banked at a time.
```
Zone "Buttons"
    BankLeft                    TrackBank -8
    BankRight                   TrackBank 8
    ChannelLeft                 TrackBank -1
    ChannelRight                TrackBank 1
ZoneEnd
```

## SelectedTrackBank
This action works similarly to TrackBank but selects the track in Reaper. Use this in a selected track style workflow.
```
Zone "Buttons"
    BankLeft                    SelectedTrackBank -8
    BankRight                   SelectedTrackBank 8
    ChannelLeft                 SelectedTrackBank -1
    ChannelRight                SelectedTrackBank 1
ZoneEnd
```

**Tip:** If you would prefer a workflow where you can bank your multi-fader surface but also have the track selection in Reaper updated each time you do, then you can combine both actions, one right after the other as shown below.
```
Zone "Buttons"
    BankLeft                    TrackBank -8
    BankLeft                    SelectedTrackBank -8
    BankRight                   TrackBank 8
    BankRight                   SelectedTrackBank 8
    ChannelLeft                 TrackBank -1
    ChannelLeft                 SelectedTrackBank -1
    ChannelRight                TrackBank 1
    ChannelRight                SelectedTrackBank 1
ZoneEnd
```
