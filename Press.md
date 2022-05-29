Message Generators that send a message when pressed, and optionally send another message when released. 

Defined using the following syntax:     
````Press 90 5e 7f 90 5e 00````      

where:
* 90 5e 7f is the message sent when the widget is pressed
* 90 5e 00 is the message sent when the widget is released (optional)

### Widget Definitions
````
Widget Play
	Press 90 5e 7f
WidgetEnd
````

The Play Widget has been declared without a Release message -- by far the most common definition type.

````
Widget Shift
	Press 90 46 7f 90 46 00
WidgetEnd
````

The Shift Widget has been declared with a Release message -- good for Buttons you plan to use for Shift/Option/Control/Alt, etc. or if you wish to use the "Hold" modifier.

### What is AnyPress ?
A Message Generator for use with surfaces whose buttons alternate between a press message (7f) on press, and on second press, a release message (00). Use AnyPress widgets for these types of surfaces. 

```` 
Widget BankLeft
	AnyPress 90 2e 7f
WidgetEnd
````

In case you're still not sure about when to use Press versus AnyPress, try this:

1. Run the Reaper Action "CSI Toggle Show Input from Surfaces"
2. Press the button on your surface once

**Result:**

If you see two messages like 90 2e 7f and 90 2e 00, then use a Press widget.

If you only see one message like 90 2e 7f, then press the button a second time...

* If the next message is also 90 2e 7f, then use a Press widget with a single message defined.

* If the next message is 90 2e 00, then you should use an AnyPress widget.

### Stepping Through Parameters
If you want to step through a parameter using button presses, that can be done similar to how we can map [[Encoders|Encoder]] to stepped parameters. The VST parameters are in a range of 0 to 1, and the parameter steps can be entered in brackets (with a space after the opening bracket and before the closing one).

In this example, each "SomeButton" press will increment the tape type in IK Multimedia's Tape machines forward. And just to illustrate, when combined with a Shift modifier, you can increment the buttons in the opposite direction. 
```` 
SomeButton FXParam "3" "TapeType" [ 0.0 0.33 0.67 1.0 ]
Shift+SomeButton FXParam "3" "TapeType" [ 1.0 0.67 0.33 0.0 ] 
````

The parameter steps can even be in any order you want!  You could group like items together, like if you had ValhallaVintage verbs and wanted all the hall modes next to one another. You could even skip steps: let's say there's a mode in a plugin you hate, you could leave that step out entirely!    

Note: When incrementing steps upwards, CSI will return to the bottom of the list after the highest step. When incrementing downwards, CSI will stop at the bottom step. This behavior exists to prevent unwanted, potential jumps in volume. 
