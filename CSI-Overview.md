The Control Surface Integrator (CSI) is a Reaper plugin that aims to let you integrate your hardware control surfaces into Reaper with a much higher level of control than is possible out of the box. If you're prepared to put in the effort to customize your configuration, you will be able to integrate one or more MIDI, MCU, or OSC, control surfaces into a single virtual surface.

Note: CSI requires access to the MIDI Devices and needs them disabled in Reaper's Preferences>Audio>MIDI Devices. For this reason, CSI may not be ideal for use with MIDI Controller Keyboards that will require notes to be passed along to Reaper in addition to the control functions. However, users have successfully implemented work arounds by using BOME MIDI Translator to split out note versus control data and creating a virtual MIDI port for use with CSI and the control data.

## Benefits of CSI

* Works with any MIDI, MCU or OSC device
* Allows multiple surfaces to work as an integrated system
* Support files include starting points for many common surfaces
* Highly customizable (text based surface files have a relatively small learning curve)
* Mac and PC support
* Open-source project (contributions accepted)

## High-level Concepts

### Pages
Within CSI you can define one or more [[Pages|Pages]], with each Page containing the configuration information for whatever Surfaces and Actions you want CSI to recognise. Only one Page can be active in Reaper at any time, but you can switch between [[Pages|Pages#paging-actions]] easily. So for example, you might have a Recording Page and a Mixing Page, with the physical elements on your Surfaces (eg. buttons, faders, etc) mapped to different actions in each. You can have surfaces included, left out, or even defined completely differently on each different Page.

### Surfaces
Each Surface within your page is represented by 2 major pieces:

* [[mst/ost file|Defining Control Surface Capabilities]]- the Surface Template file, which specifies the Surface's capabilities. ie. what elements it contains (eg. buttons, faders, encoders, lights, displays, etc), and what MIDI/OSC messages it sends and expects to receive.
* [[Zone files|Defining Control Surface Behavior]] - Zone files can define a couple of different behaviors:
  * [[Controlling Reaper|Zones]] - how the Surface elements defined in the mst/ost file are mapped to Actions
  * [[Controlling Plugins|FX-and-Instrument-Mapping]] - how the Surface elements defined in the mst/ost file will map to parameters in your VST plugins

