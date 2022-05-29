Encoders are typically referred to as "Endless Rotary" knobs (i.e. knobs with no absolute begin and end positions). While CSI typically requires the use of [[Fader7Bit]] widgets for traditional rotary knobs that communicate absolute start and end values, Encoders are different in that they communicate a relative change in value (i.e. it sends one value when it is increasing and one when it is decreasing).

One large benefit of encoders versus traditional Rotary knobs that would use a [[Fader7Bit]] widget, is that encoders typically allow for much higher resolution than 7bit MIDI.

In order to support the widest possible range of hardware encoders, CSI now allows users to customize which values their encoders transmit as well as define acceleration ranges for encoders that support it. EncoderPlain and EncoderPlainReverse are available if your surface has no acceleration.

# Encoder Types

## MCU Encoder
Here's an example of a typical MCU **Encoder** widget in CSI.
```
Widget Rotary1
	Encoder b0 10 7f
	FB_Encoder b0 10 7f
WidgetEnd
```

## Non-MCU Encoders, No Acceleration
Use this syntax only if your hardware encoder transmits a single-value for clockwise and counter-clockwise turns. Because each surface may be different, replace the "41" and "01" messages with the correct values for your hardware.

In this example, the hardware encoder only transmits a value of 41 when rotated counter-clockwise (decrement, as noted by the < symbol), and only transmits a value of 01 when being rotated clockwise (increment, as noted by the > symbol). 
```
Widget Rotary1
	Encoder b0 10 7f [ < 41 > 01 ]
	FB_Encoder b0 10 7f
WidgetEnd
```

## Encoder With Discrete Acceleration Steps
Use this syntax when surface transmits different acceleration values for increment (CW) and decrement (CCW) turns. However, your surface may skip values (notice 48 and 49 are missing as are 08 09).
```
Widget Rotary1
	Encoder b0 10 7f [ < 41 42 43 44 45 46 47 4a > 01 02 03 04 05 06 07 0a ]
	FB_Encoder b0 10 7f
WidgetEnd
```

## Encoders With a Continuous Acceleration Range
Use this syntax when your encoder transmits a continuous range of acceleration values between a defined start and end range with no breaks or jumps in the data. 

```
Widget Rotary1
	Encoder b0 10 7f [ < 41-4a > 01-0a ]
	FB_Encoder b0 10 7f
WidgetEnd
```

## MIDI Fighter Twister Encoders (MFTEncoder)
The MIDI Fighter Twister by DJ Tech Tools can be configured to work as an Encoder with Velocity Acceleration. However, because it uses a non-standard set of values when turning the knob clockwise and counter-clockwise, a special widget was developed for the MIDI Fighter Twister. The correct syntax for a MFTwister encoder is shown below, including the full set of acceleration steps when using the Velocity Sensitive encoder setting. As you can see, there are 11 acceleration levels in each direction.

More details and detailed explanation is provided on the [[MFTEncoder]] page.

```
Widget RotaryA1
	MFTEncoder b0 00 7f [ < 3f 3e 3d 3c 3b 3a 39 38 36 33 2f > 41 42 43 44 45 46 47 48 4a 4d 51 ]
	FB_Fader7Bit b0 00 00
WidgetEnd
```

# Zone File Custom Parameter Ranges, Deltas, and Acceleration
CSI now also allows you to customize the parameter ranges for a given control, how quickly an encoder will step through a parameter (the delta), and even customize the acceleration curve for an encoder. All on a per-parameter basis!

## Default Range, Default Step Size, Default Acceleration
If you would rather rely on CSI's default values for encoders, just map your FX Parameter or CSI action to your encoder widget exactly like you normally would any CSI action. The full 0.0 to 1.0 parameter range will be used, as will the default CSI delta, with absolutely no acceleration. 

```
Encoder1 FXParam 1 "Attack" 
```

## Custom Parameter Ranges
Now, let's say you do not want to map your encoder to the full range of a particular plugin. Instead, you want to limit the range to only the first 66% of the parameter's full range. Example: you have a compressor with a very wide range of time for attack and release and you've determined only the first two-thirds of the range is usable. You can now limit the parameter ranges in CSI with the below syntax.

```
Encoder1 FXParam 1 "Attack" [ 0.0>0.66 ]
```


## Custom Encoder Tick Size (Custom Delta)
Let's say you don't necessarily want acceleration for a particular parameter, but you do want to tweak how much each encoder tick changes the parameter value (i.e. the delta). You can define how fine or coarse you want each tick of the encoder to be. In this case, we've changed the step size to 0.003 (or .3%).

```
Encoder1 FXParam 1 "Attack" [ (0.003) ]
```

## Custom Acceleration Curves
In this example, we've added a custom acceleration curve. To begin to understand this, consider that all parameters in CSI have a range from 0.0 to 1.0 (except for DB based Actions, but forget those for now). So what we're seeing here is the range of change values (or "deltas") for the encoder acceleration you defined earlier in your widget. The slowest turn will result in a change of 0.001 (.1%) which is very fine, and the fastest turn you defined will result in a step change of 0.1 (or 10%). 

What makes this so powerful is that you can completely adjust these values to fit your preference at a per-parameter basis. 

```
Encoder1 FXParam 1 "Attack" [ (0.001,0.005,0.025,0.05,0.1) ]   
```

## Custom Parameter Range With Custom Step Size
You can even combine the examples above. In this example, we're only using the first two-thirds of the parameter, but we're using our custom-defined step size of 0.003 (or .3%) per encoder tick.

```
Encoder1 FXParam 1 "Attack" [ 0.0>0.66 (0.003) ]
```

## Custom Parameter Range With Acceleration
Here's an example of how to limit the parameter range AND use a custom acceleration curve.

```
Encoder1 FXParam 1 "Attack" [ 0.0>0.66 (0.001,0.0025,0.033,0.05) ]
```

# Stepped Parameters

Because encoders don't have absolute begin and end values like a fader, this presents a challenge when mapping stepped FX Parameters to encoder widgets. What's a stepped parameter? Example: an SSL BusCompressor has stepped attack times: .1ms, .3ms, 1ms, 3ms, 10ms, 30ms. The knob will jump to those values with no in-between settings possible. For these types of controls, CSI needs to be told in the fx.zon file, where exactly each parameter step is located. The lowest possible value is always 0.0 and the highest possible value is always 1.0. 

## Mapping Stepped Parameters To Encoders
In this FX zone, there are 3 Character modes, whose parameter values correspond to 0.0, 0.5, and 1.0, there are 10 Ensemble options (which are .1 step apart), and bypass is a simple on/off toggle (either 0.0 or 1.0). So to make this work with our Encoder widgets in an fx.zon file, they need to be mapped in our .zon file as such:

```
Zone "VST: Sonsig Rev-A (Relab Development)"
FocusedFXNavigator
Encoder1 FXParam "3" "Character" [ 0.0 0.5 1.0 ]
Encoder2 FXParam "4" "Ensemble" [ 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0 ]
Encoder3 FXParam "0" "Bypass" [ 0.0 1.0 ]
ZoneEnd
```

## Overriding the Tick Count on Stepped Parameters
When using stepped parameters, you may find that you want to specify more, or fewer, encoder "ticks" to move the parameter to the next defined step. This is great for decreasing (or increasing) the sensitivity of the encoder when combined with stepped parameters. For instance, let's say a very small turn of your encoder is moving your parameter from the minimum to maximum values far too quickly. You may want to slow that down.

In this example, the "(12)" defines the number of encoder ticks (messages) that CSI must receive before it will move to the next parameter step. For example, if you're turning your encoder clockwise, CSI will need to count 12 consecutive increase values before the parameter moves from 0.0 to 0.1. Another 12 increase values will move it from 0.1 to 0.2, and 12 counter-clockwise messages will move it back down from 0.2 to 0.1.

```
Encoder2 FXParam 4 "Ensemble" [ (12) 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0 ]
```

## Using Acceleration With Stepped Parameters
You can even create custom encoder acceleration values to work with stepped parameters. Looking at the below example, we see a list in parenthesis, followed by our traditional stepped parameters outside of the parenthesis. The values in the parenthesis relate to the encoder speed defined in your widget. So the slowest acceleration value will require 20 encoder ticks to move up or down to the next step, and fastest encoder acceleration will require 3 encoder ticks to move to the next parameter. 

```
Encoder1 FXParam  "4" "Ensemble" [ (20,15,12,6,3) 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1.0 ]
```

Note: if your surface transmits 5 values for acceleration, the list in parenthesis must have the same number of values values as your hardware. So if your encoder transmits 11 acceleration values, the list in parenthesis should be 11 values long, etc. This way the acceleration speed always corresponds to a value produced by the hardware. Copy and paste is your friend.

# Additional Notes

- Some encoders like the MIDI Fighter Twister or Behringer BCR-2000 may require a FB_Fader7Bit processor instead of FB_Encoder to work properly. If you're not getting feedback, or your encoder is otherwise not working, you may want to try FB_Fader7Bit.

- When trying to determine the values, don't pay attention to what is displayed on the plugin's GUI. Example: a decay time of 2.4 seconds will **not** equal a parameter value of 2.4. One trick is to enter Write mode and begin creating automation data for the parameter you're trying to map, then clicking on the nodes and making note of where the parameter steps jump and what their real automation values are (remember: they will be between 0.0 and 1.0).


- When trying to decide if a Surface control should be represented using [[Fader7Bit]] or [[Encoder]], look at what values are actually being sent by the Surface. [[Fader7Bit]] expects an absolute value, whereas [[Encoder]] expects an increment/decrement value.

    *Use the Reaper Action "CSI Toggle Show Input from Surfaces" to see what's actually being sent.*     
    *If you see a single value being sent when you turn the control clockwise and another when the control is turned anti-clockwise, go with Encoder.*    
    *If you see a range of values sent when you turn the control (increasing in one direction, decreasing in the other) go with Fader7Bit.*    