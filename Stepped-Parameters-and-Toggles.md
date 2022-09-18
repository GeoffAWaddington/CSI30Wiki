This page will cover mapping toggles and stepped parameters to buttons. Visit the [Encoders](https://github.com/GeoffAWaddington/CSIWiki/wiki/Message-Generators#Encoders) page for details on how to map stepped parameters to an endless rotary encoder.

# Toggles [ Two Steps ]

Any on/off type of parameter needs to be mapped in CSI using a syntax where the off position is normalized to a value of 0.0 and the on position is normalized to a value of 1.0. These get placed in brackets following the parameter or CSI action. 

The below example illustrates the syntax for a simple on/off toggle mapped to a button:
```
SomeButton     FXParam 10 [ 0.0 1.0 ]
```
As your about to discover, this type of syntax is the same regardless of how many steps you have.

# Parameter Resets [ One Step ]
You can even use the same syntax above to reset parameters to a particular default value that you define in the zone file. This can be a very efficient way to reset your pans, volume, input/output gains or re-center bipolar FX knobs in a plugin.zon file, etc.

In this example, Shift+Select will reset the fader to unity gain, RotaryPush will reset the pan to center, Shift+RotaryPush will reset the Pan Width to the maximum width, and so on for Pan Left and Pan Right.
```
Select|              TrackUniqueSelect
Fader|               TrackVolume        
Shift+Select         TrackVolume [ 0.716 ]  // Sets fader to unity gain
Rotary|              TrackPan "1"
RotaryPush|          TrackPan [ 0.5 ]       // Centers Pan
Shift+Rotary|        TrackPanWidth
Shift+RotaryPush|    TrackPanWidth [ 1.0 ]  // Maxes out the track width
Option+Rotary|       TrackPanL 
Option+RotaryPush|   TrackPanL [ 0.0 ]      // Full left
Rotary|              TrackPanR
Control+RotaryPush|  TrackPanR [ 1.0 ]      // Full right
```

# Stepped Parameters [ Multiple Steps ]
Mapping multiple steps is essentially the same as mapping a toggle, there are just more steps that need to be added to the list. When you're mapping multiple steps an ascending list will loop back to the beginning when it reaches the end of the list. A descending list will not loop. This is by design to avoid someone accidentally blasting themselves with volume if they assign a limiter's gain to stepped parameter list and suddenly go from silence [ 0.0 ] position to full blast [ 1.0 ] when they cycle through the list.

```
Zone "VST: TR5 Tape Machine 80 (IK Multimedia)" "Tape Machine 80"
Button1  FXParam 3 [ 0.0 0.34 0.67 1.0 ]      // Step up a list: this WILL loop
Button2  FXParam 3 [ 1.0 0.67 0.34 0.0 ]      // Step down: this will NOT loop
ZoneEnd
```

You can also use an Encoder to scroll through stepped parameters. Here, I'm using Rotary 1 (an encoder widget) to cycle through the stepped parameters.
```
Zone "VST: TR5 Tape Machine 80 (IK Multimedia)" "Tape Machine 80"
Rotary1  FXParam 3 [ 0.0 0.34 0.67 1.0 ]
ZoneEnd
```

For more information on assigning Stepped Parameters to encoders in CSI see the section on [Encoder Customization.](https://github.com/GeoffAWaddington/CSIWiki/wiki/Message-Generators#encoder-customization)

## Stepped Parameter Reference Table
Most developers will evenly distribute and normalize the number of parameter steps in their plugins. This won't always be true, but I'd say it's the case for around 98% of stepped parameters. As a result, you can try to reference the below list of parameter step sizes. Remember: only copy the stuff in the brackets, not the number that precedes them.

```
# of Steps: [ values ]
2:    [ 0.00 1.00 ]                                                                                                                                            
3:    [ 0.00 0.50 1.00 ]                                                                                                                                       
4:    [ 0.00 0.33 0.67 1.00 ]                                                                                                                                  
5:    [ 0.00 0.25 0.50 0.75 1.00 ]                                                                                                                             
6:    [ 0.00 0.20 0.40 0.60 0.80 1.00 ]                                                                                                                        
7:    [ 0.00 0.17 0.33 0.50 0.67 0.83 1.00 ]                                                                                                                   
8:    [ 0.00 0.14 0.29 0.43 0.57 0.71 0.86 1.00 ]                                                                                                              
9:    [ 0.00 0.13 0.25 0.38 0.50 0.63 0.75 0.88 1.00 ]                                                                                                         
10:   [ 0.00 0.11 0.22 0.33 0.44 0.56 0.67 0.78 0.89 1.00 ]                                                                                                    
11:   [ 0.00 0.10 0.20 0.30 0.40 0.50 0.60 0.70 0.80 0.90 1.00 ]                                                                                               
12:   [ 0.00 0.09 0.18 0.27 0.36 0.45 0.55 0.64 0.73 0.82 0.91 1.00 ]                                                                                          
13:   [ 0.00 0.08 0.17 0.25 0.33 0.42 0.50 0.58 0.67 0.75 0.83 0.92 1.00 ]                                                                                     
14:   [ 0.00 0.08 0.15 0.23 0.31 0.38 0.46 0.54 0.62 0.69 0.77 0.85 0.92 1.00 ]                                                                                
15:   [ 0.00 0.07 0.14 0.21 0.29 0.36 0.43 0.50 0.57 0.64 0.71 0.79 0.86 0.93 1.00 ]                                                                           
16:   [ 0.00 0.07 0.13 0.20 0.27 0.33 0.40 0.47 0.53 0.60 0.67 0.73 0.80 0.87 0.93 1.00 ]                                                                      
17:   [ 0.00 0.06 0.13 0.19 0.25 0.31 0.38 0.44 0.50 0.56 0.63 0.69 0.75 0.81 0.88 0.94 1.00 ]                                                                 
18:   [ 0.00 0.06 0.12 0.18 0.24 0.29 0.35 0.41 0.47 0.53 0.59 0.65 0.71 0.76 0.82 0.88 0.94 1.00 ]                                                            
19:   [ 0.00 0.06 0.11 0.17 0.22 0.28 0.33 0.39 0.44 0.50 0.56 0.61 0.67 0.72 0.78 0.83 0.89 0.94 1.00 ]                                                       
20:   [ 0.00 0.05 0.11 0.16 0.21 0.26 0.32 0.37 0.42 0.47 0.53 0.58 0.63 0.68 0.74 0.79 0.84 0.89 0.95 1.00 ]                                                  
21:   [ 0.00 0.05 0.10 0.15 0.20 0.25 0.30 0.35 0.40 0.45 0.50 0.55 0.60 0.65 0.70 0.75 0.80 0.85 0.90 0.95 1.00 ]                                             
22:   [ 0.00 0.05 0.10 0.14 0.19 0.24 0.29 0.33 0.38 0.43 0.48 0.52 0.57 0.62 0.67 0.71 0.76 0.81 0.86 0.90 0.95 1.00 ]                                        
23:   [ 0.00 0.05 0.09 0.14 0.18 0.23 0.27 0.32 0.36 0.41 0.45 0.50 0.55 0.59 0.64 0.68 0.73 0.77 0.82 0.86 0.91 0.95 1.00 ]                                   
24:   [ 0.00 0.04 0.09 0.13 0.17 0.22 0.26 0.30 0.35 0.39 0.43 0.48 0.52 0.57 0.61 0.65 0.70 0.74 0.78 0.83 0.87 0.91 0.96 1.00 ]                              
25:   [ 0.00 0.04 0.08 0.13 0.17 0.21 0.25 0.29 0.33 0.38 0.42 0.46 0.50 0.54 0.58 0.63 0.67 0.71 0.75 0.79 0.83 0.88 0.92 0.96 1.00 ]                         
26:   [ 0.00 0.04 0.08 0.12 0.16 0.20 0.24 0.28 0.32 0.36 0.40 0.44 0.48 0.52 0.56 0.60 0.64 0.68 0.72 0.76 0.80 0.84 0.88 0.92 0.96 1.00 ]                    
27:   [ 0.00 0.04 0.08 0.12 0.15 0.19 0.23 0.27 0.31 0.35 0.38 0.42 0.46 0.50 0.54 0.58 0.62 0.65 0.69 0.73 0.77 0.81 0.85 0.88 0.92 0.96 1.00 ]               
28:   [ 0.00 0.04 0.07 0.11 0.15 0.19 0.22 0.26 0.30 0.33 0.37 0.41 0.44 0.48 0.52 0.56 0.59 0.63 0.67 0.70 0.74 0.78 0.81 0.85 0.89 0.93 0.96 1.00 ]          
29:   [ 0.00 0.04 0.07 0.11 0.14 0.18 0.21 0.25 0.29 0.32 0.36 0.39 0.43 0.46 0.50 0.54 0.57 0.61 0.64 0.68 0.71 0.75 0.79 0.82 0.86 0.89 0.93 0.96 1.00 ]     
30:   [ 0.00 0.03 0.07 0.10 0.14 0.17 0.21 0.24 0.28 0.31 0.34 0.38 0.41 0.45 0.48 0.52 0.55 0.59 0.62 0.66 0.69 0.72 0.76 0.79 0.83 0.86 0.90 0.93 0.97 1.00 ]
```

**Important tip from Mixmonkey:**
> If this doesn't work out and the control 'gets stuck' at a particular step [this is usually a rounding difference], try increasing the next step's value by a small amount, say 0.01 or 0.02.

> Occasionally you come across plugins where the values are completely skewed and in those cases you'll have to do your best to work out what the steps actually are. I use BlueCat Patchwork, as it lets you assign a parameter to a control and then change the value in 1% increments, so you can see where the parameter changes.