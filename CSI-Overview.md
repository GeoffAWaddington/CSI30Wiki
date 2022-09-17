The Control Surface Integrator (CSI) is a Reaper plugin that aims to let you integrate your hardware control surfaces into Reaper with a much higher level of control than is possible out of the box. If you're prepared to put in the effort to customize your configuration, you will be able to integrate one or more MIDI, MCU, or OSC, control surfaces into a single virtual surface.

Note: CSI requires access to the MIDI Devices and needs them disabled in Reaper's Preferences>Audio>MIDI Devices. For this reason, CSI may not be ideal for use with MIDI Controller Keyboards that will require notes to be passed along to Reaper in addition to the control functions. However, users have successfully implemented work arounds by using BOME MIDI Translator to split out note versus control data and creating a virtual MIDI port for use with CSI and the control data.

## Benefits of CSI

* Works with any MIDI, MCU or OSC device
* Allows multiple surfaces to work as an integrated system
* Control Reaper and plugins
* Support files include starting points for many common surfaces and example FX mappings
* Highly customizable (text-based surface files have a relatively small learning curve)
* Includes color support on X-Touch, FaderPort8/16, MIDI Fighter Twister, and OSC surfaces
* Can create custom encoder response curves
* Mac and PC support (no Linux)
* Open-source project (contributions accepted)

## High-level Concepts

### Pages
Within CSI you can define one or more [[Pages|Pages]], with each Page containing the configuration information for whatever Surfaces and Actions you want CSI to recognise. Only one Page can be active in Reaper at any time, but you can switch between [[Pages|Pages#paging-actions]] easily. So for example, you might have a Recording Page and a Mixing Page, with the physical elements on your Surfaces (eg. buttons, faders, etc) mapped to different actions in each. You can have surfaces included, left out, or even defined completely differently on each different Page.

### Surfaces
Each Surface within your page is represented by 2 major pieces:

* The Surface Template file: [[mst/ost file|Defining Control Surface Capabilities]]
  * The MIDI Surface Template (.mst) and OSC Surface Template (.ost) files define the control surface capabilities
  * Each control and display (a.k.a. [[Message Generators]]) on the surface is represented as a "Widget"
  * Each Widget defines what messages CSI expects to receive from the surface and
  * [[Feedback Processors]] in the Widget tell CSI which messages to send to the surface
* [[Zone files|Defining Control Surface Behavior]]
  * Dictate how the Control Surface is expected to map to Reaper elements and/or FX/Instrument plugins
  * [[Controlling Reaper|Zones]] - how the Surface elements defined in the mst/ost file are mapped to Actions
  * [[Controlling Plugins|FX-and-Instrument-Mapping]] - how the Surface elements defined in the mst/ost file will map to parameters in your VST plugins

