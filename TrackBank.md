When we have multiple faders set up to map to our tracks (such as we have in our [[Channel zone example in the Zones page|Zones#a-slightly-more-useful-example]], we may want to have buttons set up to bank left and right through the tracks in Reape using the TrackBank Action:

````
LeftButton TrackBank "-16" 
RightButton TrackBank "16"
````

The parameter (16 in the example above) indicates how may tracks left (-) or right (+) you'd like to shift.

## Example
If that's a bit confusing, let me give you an example. I have two BCF2000 surfaces set up, each of which has 8 faders. So in total I can map 16 tracks in Reaper across these faders. What happens when I have more than 16 tracks in Reaper? Well, my 16 tracks on my surfaces become like a window onto my tracks in reaper, a window that I can move around to show any 16 tracks. 

So at the start I may have Tracks 1-16 mapped to my surfaces. If I my RightButton above, it'll shift to map tracks 17 - 32 across my surfaces. If I then press LeftButton, it'll shift back to map tracks 1-16 across the surfaces.

It will also deal with irregular numbers of tracks. If I have 36 tracks in Reaper. I'll start with tracks 1-16, then RightButton will take me to 17-32. RightButton again will map 21-36, as there are no more tracks to map. Pressing LeftButton a this point will map 5-20, and LeftButton again will take me back to 1-16.

You can also set up buttons that just adjust by one like this:

````
Shift+LeftButton TrackBank "-1" 
Shift+RightButton TrackBank "1"
````
