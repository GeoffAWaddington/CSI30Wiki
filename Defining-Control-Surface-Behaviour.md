We define what behaviour should occur when particular widgets are pressed/turned/moved/etc in .zon files. 

If you remember in our [[CSI.INI]] file, for each surface we listed a number of things:

`MidiSurface AlphaTrack 8 8 AlphaTrack.mst AlphaTrack 1 0 0 0 `

In this example, the AlphaTrack.mst was where we defined what widgets were on the surface. 

The next item to the right, in this case 'AlphaTrack' is the name of a Zones folder. Inside this folder, we should create whatever .zon files we want that relate to the widgets on this surface. We can also use subfolders, and subfolders of subfolders and so on, in order to organize our .zon files.

CSI will read in all the .zon files it finds in this folder, including all subfolders, so don't store backups/unused here, they WILL get pulled in. 

Note: CSI expects ONE ZONE ONLY PER FILE. 

Inside each .zon file we create a Zone to map the widgets on this surface to the actions we want to trigger. As you'll see on [[that page|Zones]], we can also switch between currently active zones, compose zones out of other zones, etc. 

## Types of Zones

At this stage there are primarily two types of Zones that we can define:
* [[Zones that control general Reaper functionality|Zones]]. eg. the Tracks in our project, the Transport controls, our Track Sends, etc
* [[Zones that control VST instruments/FX inside Reaper|FX-and-Instrument-Mapping]]. eg. How to map the parameters of your EQ, Reverb, etc on the selected track across your surfaces.

They are both defined as Zones in .zon files, but there are some additional details for [[FX Zones that are worth discussing|FX-and-Instrument-Mapping]]. 