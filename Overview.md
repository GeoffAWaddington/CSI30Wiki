## Overview 
The CSI Project aims to let you integrate your hardware control surfaces into Reaper with a much higher level of control than is possible out of the box. If you're prepared to put in the effort to customise your configuration, you should be able to integrate one or more control surfaces (Surfaces from now on) into a single virtual Surface.

It supports MIDI, MCU and OSC. Eucon was supported in prior builds, but was removed as of CSI version 1.1. See this link for information on the [new Eucon project](https://forum.cockos.com/showthread.php?t=255955).

PC and Intel Mac systems are supported. A recent version was also built for M1 Macs (Apple Silicon). There is no Linux build at this time but the source code is available on this github if anyone would like to attempt a Linux port.. See the [Installation](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/Installation) page for links. 

## High-level Concepts

### Pages
Within CSI you can define one or more [[Pages|Pages]], with each Page containing the configuration information for whatever Surfaces and Actions you want CSI to recognise. Only one Page can be active in Reaper at any time, but you can switch between [[Pages|Pages#paging-actions]] easily. So for example, you might have a Recording Page and a Mixing Page, with the physical elements on your Surfaces (eg. buttons, faders, etc) mapped to different actions in each. You can have surfaces included, left out, or even defined completely differently on each different Page.

### Surfaces
Each Surface within your page is represented by 2 major pieces:

* [[mst/ost file|Defining Control Surface Capabilities]]- the Surface Template file, which specifies the Surface's capabilities. ie. what elements it contains (eg. buttons, faders, encoders, lights, displays, etc), and what MIDI/OSC messages it sends and expects to receive.
* [[Zone file|Defining Control Surface Behaviour]] - Zone files can define a couple of different behaviours:
  * [[Controlling Reaper|Zones]] - how the Surface elements defined in the mst/ost file are mapped to Actions
  * [[Controlling Plugins| FX Zones]] - how the Surface elements defined in the mst/ost file will map to parameters in your VST plugins

