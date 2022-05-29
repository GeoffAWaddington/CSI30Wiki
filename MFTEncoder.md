# Midi Fighter Twister Encoder
MFTEncoder is a message generator specifically for any of the knobs of the Midi Fighter Twister.

The Midi Fighter Twister is a four by four twisted knob unit where each knob has a push functionality, plus six buttons on the left- and right-hand sides of the unit. It can be setup with the MF Utility provided by DJ Techtools to send out different types of data.

Encoders are knobs that are configured to send out relative control data, not absolute values. They are meant to increase or decrease the value of the parameter they are meant to control. As the Midi Fighter Twister sends out absolute control data by default, the unit must be reconfigured with the MF Utility by DJ Techtools.

The relative control of Midi Fighter Twister knobs configured as encoders come in three varieties, one of which allows for acceleration values to be included. This is the mode we will focus on here. The unit can be configured to use acceleration but CSI can ignore them and treat them as non-acceleration values.

## Configuring the knobs to be encoders - necessary settings
### Encoder MIDI Type    _ENC 3FH/41H_
Relative control data instead of absolute control data is sent

Sensitivity  _Velocity Sensitive_
This sets up acceleration. Instead of just the hex values 3Fh and 41h being used to indicate decrease and increase, a range of values are not used.

## Configuring the knobs to have switch functionality
### Switch Action Type :     _Note Hold_
The push action of each knob will now send out a NOTE ON message on pressing down and a NOTE OFF message when letting go. The _Note Toggle_ would alternate between the NOTE ON and NOTE OFF message with each press, which can be useful to toggle between two absolute value for a parameter but the same can be acomplished with just one MST widget and one action in the zone file.

## Using the MFTEncoder widget in an MST file
```
Widget RotaryA1
    MFTEncoder   b0 00 7f [ < 3f 3e 3d 3c 3b 3a 39 38 36 33 2f > 41 42 43 44 45 46 47 48 4a 4d 51 ]
    FB_Fader7Bit b0 00 7f
WidgetEnd
```
***
```
RotaryA1
```
The name we give this encoder control source.


```
MFTEncoder
```
The message generator.


```
b0 00 7f
```

The data it sends.

To know what to place in the three bytes afer the _MFTEncoder_ statement, you can easily look at the data the encoder sends by using MIDI monitor software, such as Midi OX for Windows or an equivalent tool for OSX or Linux. The first two bytes will always be identical for each encoder. The third byte is always the data. CSI expects the value **7F** in the widget description.

```
[ < 3f 3e 3d 3c 3b 3a 39 38 36 33 2f > 41 42 43 44 45 46 47 48 4a 4d 51 ]
```
This describes the acceleration values the encoder can send and whether they represent an increase in value or decrease for the intended parameter. 

```
FB_Fader7Bit b0 00 7f
```
The feedback generated from the value of the parmeter being controlled. The Midi Fighter Twister expects 7-bit midi data for the value, on the same channel (0 as midi data counts midi channels from 0 to 15) and for the save CC value ( 00 in this case). 7f is a place holder.


With the message generator in place in the MST file, the zone file will make proper use of this data.

# Using MFTEncoder data in a ZONE file
## FX Zones
Using encoders in general is described in great detail on the [[Encoders]] page.

An example
```
RotaryA1 FXParam 0 "Thresh" [ 0<0.8 (0.001,0.001,0.001,0.001,0.002,0.002,0.002,0.003,0.003,0.004,0.004) ]
```
```RotaryA1``` - name of the message generator assigned in the MST file.

```FXParam``` - a paramameter of an insert effect is to be controlled

```0``` - the first parameter is to be controlled. Parameters are counted starting with zero.

```"Thresh"``` - a name we can arbitrarily call this parameter. It is optional but useful to keep them apart.

```[ ]``` - encloses any range and acceleration handling

```0<0.8``` - the value of the parameter should stay between 0 and 0.8. All VST/AU/VST3/LV2 plugin parameters usually go from 0 to 1.0. The plugin may present them differently but that's what goes on in the background. This is optional. Without such a range statement CSI will assume a range of 0 to 1.0.

```(0.001,0.001,0.001,0.001,0.002,0.002,0.002,0.003,0.003,0.004,0.004)```

This assigns a step value for each acceleration value for either direction described in the MFTEncoder widget in the MST file.

There are **11 values** described for the left turning on the knob and **11 values** for the right turning on the knob.
Thus we need to provide **11 values** in this zone file.

At this time the minimum step value is 0.001. This may change in the future.

The step values assignemnt for each acceleration value is optional. If none is provided, a default step value is used.