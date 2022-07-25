## CSI Overview 

## High-level Concepts

### Pages
Within CSI you can define one or more [[Pages|Pages]], with each Page containing the configuration information for whatever Surfaces and Actions you want CSI to recognise. Only one Page can be active in Reaper at any time, but you can switch between [[Pages|Pages#paging-actions]] easily. So for example, you might have a Recording Page and a Mixing Page, with the physical elements on your Surfaces (eg. buttons, faders, etc) mapped to different actions in each. You can have surfaces included, left out, or even defined completely differently on each different Page.

### Surfaces
Each Surface within your page is represented by 2 major pieces:

* [[mst/ost file|Defining Control Surface Capabilities]]- the Surface Template file, which specifies the Surface's capabilities. ie. what elements it contains (eg. buttons, faders, encoders, lights, displays, etc), and what MIDI/OSC messages it sends and expects to receive.
* [[Zone files|Defining Control Surface Behaviour]] - Zone files can define a couple of different behaviours:
  * [[Controlling Reaper|Zones]] - how the Surface elements defined in the mst/ost file are mapped to Actions
  * [[Controlling Plugins|FX-and-Instrument-Mapping]] - how the Surface elements defined in the mst/ost file will map to parameters in your VST plugins

