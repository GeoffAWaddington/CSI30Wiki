As mentioned on the [[Installation]] page, the configuration of your CSI installation is ultimately stored in a file called csi.ini. 

Here's a typical CSI.ini:
```
Page "HomePage" FollowMCP NoSynchPages UseScrollLink

MidiSurface "Console1" 7 6 "Console1.mst" "Console1" 0 0 0 0 

MidiSurface "LaunchPad" 6 7 "LaunchPadMiniMK3.mst" "LaunchPadMiniMK3" 0 0 0 0 

OSCSurface "iPad" 8001 9001 "TouchOSCPad.ost" "TouchOSCPad" 0 0 0 0  192.168.2.19

OSCSurface "iPhone" 8000 9000 "TouchOSCPhone.ost" "TouchOSCPhone" 0 0 0 0  192.168.2.11

MidiSurface "A800" 10 8 "RolandA800.mst" "RolandA800" 0 0 0 0 

EuConSurface "EuCon" "EuCon" 64 8 8 0 
```

The line starting with "Page" defines a Page.

From left to right, this states that:

* There is a Page named "HomePage"
* It follows the MCP (mixer control panel) track visibility
* NoSynchPages means that this Page banks "by itself" -- if you go to another Page it will not be banked to the same location
* UseScrollLink ensures that when you bank Tracks on the surface, they are made visible in the Reaper GUI

All the lines that come after that, starting with MidiSurface, represent each of the control surfaces. 

The Channel Offset governs how the channels display. Suppose you had a Surface with 8 Faders and an Extender with 8 Faders. You could set the Surface Channel Offset to 0, and the Extender Channel Offset to 8, to get Channels 1-8 to show up on the Surface and channels 9-16 to show up on the Extender.

Let's look look at one of them:

`MidiSurface "Console1" 7 6 "Console1.mst" "Console1" 0 0 0 0 `

From left to right, this states that:
* There is a control surface named Console1
* It sends MIDI into Reaper on port 7
* It receives MIDI from Reaper on port 6
* The controls it contains are defined in a file called Console1.mst (see [[Defining Control Surface Capabilities]])
* The zones it contains are in a folder called Console1 under the Zones folder (see [[Defining Control Surface Behaviour]])
* 0 - Number of Channels - Console 1 has no Channels, an MCU has 8, etc.
* 0 - Number of Sends - Console 1 has no Sends, an MCU has 8, etc.
* 0 - Number of FX Menu Items - Console 1 has no FX Menu Items, an MCU has 8, etc.
* 0 - Channel Offset -- this tells CSI how far from the left edge of the CSI virtual console you want your surface to start

