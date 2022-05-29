Some widgets & surfaces (such as the "Select" buttons on the Faderport8 and Faderport16 or an OSC surface designed to receive track track colors) work with CSI in such a way that the surface can follow the track colors in Reaper. If you have a supported device, the syntax is the word "Track" within some curly brackets (leave a space before and after). 

Here's an example of how to get the Select buttons to light up on the Presonus Faderport8/16 using CSI: 
```
Zone "Channel"
TrackNavigator
Select| TrackUniqueSelect  { "Track" }
ZoneEnd
```