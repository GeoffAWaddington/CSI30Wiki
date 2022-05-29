
Zones are where we do two things:

* where we map the widgets defined in our mst files to the actions we want to perform when they are invoked (pressed, rotated, etc)
* where we group multiple action/widget mappings together into related groups. 

## A Simple Example

The widget/action mapping part is easy. Let's say we have a widget called PlayButton, and we want it to perform the Play action when pressed, we would simply use the following syntax in our zon file. 

````
PlayButton Play
````

There's obviously more options than just play. Have a look at the [[Action Reference]] page for more examples. 

However, we can't just have this sitting by itself. We need to put it inside a zone definition, usually along with other widgets performing related functions (say, the Stop, Record, FF, Rewind buttons, etc). At a minimum, we’d need something like this:

````
Zone Home
        PlayButton Play
ZoneEnd
````

Provided our [[CSI.INI]] and Widget definitions are setup correctly, this should allow us to control the Play button from our Surface.


## One Zone Per File (CSI 1.1 and Up)
CSI version 1.1 introduced a major change in that now you can only have one zone per file. In the past, a surface.zon file was usually comprised of multiple zones like: Home, Channel, Button, Mastertrack, Jogwheel, SelectedTrackSends. Starting with CSI 1.1, these now need to be separate files. A similar CSI 1.1 surface would now be comprised of multiple files like:

```
Home.zon
Channel.zon
Button.zon
MasterTrack.zon
JogWheel.zon
SelectedTrackSend.zon
``` 

This change immensely improves performance when you have a large number of FX.zon's but also has the added benefit of making it easier to borrow other people's zones and more easily add them to your setup. Example: if you see a SelectedTrackSendSlot.zon file that works, you can just copy and paste that into your surface folder, and just add the appropriate mapping action to your home.zon.

**Tip:** within each **\CSI\Zones\[Surface]** folder, it's probably a best practice to create an "FX Zones" sub-folder if you plan on mapping FX. This way, your surface zone files can all live together in the root of the folder, and all of your fx zones can exist in the **\CSI\Zones\[Surface]\FX Zones** folder, away from the surface files. Not a requirement, but will help keep things tidy.

## Naming Zones
Some zones in CSI are "special zones" and require an exact name in order to work. Example: if you're controlling the faders/pans/etc. on multiple tracks across an 8-channel surface, that zone must be called "Channel" or it won't work. See the section on [Navigators](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/Navigators) for more details on that.

For zones where you do have have some flexibility in the naming convention, like a "Buttons" zone, there are some rules you'll have to follow. If your Zone name has a space in it, you'll need to wrap all references to that zone in quotes (Example: if you wanted a zone called "Zoom Buttons"), otherwise, the quotes are technically not required. That said, it's probably a good idea to wrap them all in quotes, spaces or not, just so you don't have to think about it. 

Every CSI surface must have a Home zone (i.e. home.zon if using CSI v1.1 and above).

FX zone file names can be whatever you'd like them to, but the FX zone name in the .zon file itself must match the plugin name in Reaper exactly. See [FX Zones](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/FX-Zones) for more information on how to create an FX.zon.


## A Slightly More Useful Example

As great as it is to get that first action happening when you press a Surface button, let’s look at a slightly more interesting example. I might group a bunch of widgets together into a zone called Buttons, because the actions I am assigning to them are conceptually related:

````
Zone "Buttons"
	Track ToggleMapSelectedTrackFXMenu
	Send ToggleMapSelectedTrackSends
	Pan ToggleMapSelectedTrackFX
	Shift+Pan GoZone Home
	Plugin ToggleScrollLink
        ChannelLeft TrackBank "-1"
	ChannelRight TrackBank "1"
	BankLeft TrackBank "-20"
	BankRight TrackBank "20"
	Rewind Rewind
	FastForward FastForward
	Stop Stop
	Play Play
	Record Record
	F1 NextPage
ZoneEnd
````

As another example, I might define a zone called Channels which collects all my track faders and related widgets into a single group, again because for me they are all related to controlling my tracks. 
````
Zone "Channel"
	TrackNavigator
	DisplayUpper|  TrackNameDisplay
	TrackTouch+DisplayUpper|  TrackVolumeDisplay
	Rotary| MCUTrackPan
	RecordArm|  TrackRecordArm
	Solo|  TrackSolo
	Mute| TrackMute
	Select|  TrackUniqueSelect
	Shift+Select|  TrackRangeSelect
	Control+Select|  TrackSelect
	Shift+Control+Select| TogglePin
	Fader|  TrackVolume
	FaderTouch|  TrackTouch
ZoneEnd
````

Note the | character after the name of the widgets. These will get replaced with the actual number derived from the number of channels you entered during setup. It is simply a shorthand way of reducing typing. So for example if you specified 8 channels this line:

Fader|  TrackVolume

will cause this to be generated automatically by CSI

Fader1  TrackVolume

Fader2  TrackVolume

Fader3  TrackVolume

Fader4  TrackVolume

Fader5  TrackVolume

Fader6  TrackVolume

Fader7  TrackVolume

Fader8  TrackVolume


## Changing Zones
We can also change which zones are active at the moment using an Action called GoZone:

`SomeButton GoZone "AnotherZone"`

That is telling CSI to "overlay" that zone over the currently active zones. Any widgets used in AnotherZone that were previously mapped in the currently active zones  will now trigger the actions defined in the anotherZone. Any that aren't used in AnotherZone will keep performing the actions they were before we triggered [[GoZone|Zone Actions]]. 

This results in a very flexible system where can temporarily change certain control's behavior in response to whatever widgets and/or [[Virtual Widgets]] we like.

**Note:**  If a zone name contains a space, like "Selected Channel Buttons" for example, then any GoZone actions must have the zone name in quotes. As a best practice, if you always include the zone name in quotes (even when not technically required), you'll never have to worry. 

## Adding Comments to Zones
When triggering custom actions or using actions whose meanings may otherwise not be obvious, it's a good idea to add comments to your in your .zon files. CSI allows for two types of comments:

1. / Lines that begin with a forward slash are completely ignored by CSI and are good for commenting sections of code. **Hint:** these also work in .mst files.
2. // Trailing comments are preceeded by two forward slashes and can be used after an action in a .zon file

Here is an example from a .zon file that uses both types of comments. First, the entire section is preceded by a comment using a single forward slash. Then, each of the automation modes is followed by a trailing comment identifying which mode is which.

````
/To make MCU Std mode work like MCU Cubase mode, I'm using F1-F5 on the X-Touch One for the automation modes.

Zone "SelChannelButtons|"
	SelectedTrackNavigator
	F1 				TrackAutoMode 1 	//Read
	F2 				TrackAutoMode 3 	//Write
	F3 				TrackAutoMode 0 	//Trim
	F4 				TrackAutoMode 2 	//Touch
	F5				TrackAutoMode 4 	//Latch

ZoneEnd
````
