This action is meant to work for channel navigation similar to Reaper's Select Previous/Next Track action. What is the difference between this and the CSI "TrackBank" action? The typical TrackBank action will not work with a SelectedTrackNavigator (like on a one-fader surface), and SelectedTrackBank requires no navigator at all. It only requires that a track be selected in Reaper.

```
     BankLeft           SelectedTrackBank -8     // Selects 8 tracks prior from the current selected track
     BankRight          SelectedTrackBank 8      // Selects 8 tracks up from the current selected track
     ChannelLeft        SelectedTrackBank -1     // Selects the previous track form the current selected track
     ChannelRight       SelectedTrackBank 1      // Selects the next track form the current selected track
```