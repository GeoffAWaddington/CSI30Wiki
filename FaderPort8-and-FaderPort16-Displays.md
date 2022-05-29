CSI supports the Presonus FaderPort8 and FaderPort16 Displays. Each of the 4 display rows requires it's own widget in the .mst file.

For your .mst, here are the names for the FB generators that correspond to each line on the surface. 

```
FB_FP8DisplayUpper

FB_FP8DisplayUpperMiddle

FB_FP8DisplayLowerMiddle

FB_FP8DisplayLower // DisplayLower not implemented as of Dec 18 2020 - do not use.



FB_FP16DisplayUpper

FB_FP16DisplayUpperMiddle

FB_FP16DisplayLowerMiddle

FB_FP16DisplayLower // DisplayLower not implemented as of Dec 18 2020 - do not use.
```

And here is an example of how the .mst would be mapped out for Display 1 on the Faderport16.
```
Widget FPScribbleUpper1
	FB_FP16DisplayUpper "0"
WidgetEnd

Widget FPScribbleUpperMiddle1
	FB_FP16DisplayUpperMiddle "0"
WidgetEnd

Widget FPScribbleLowerMiddle1
	FB_FP16DisplayLowerMiddle "0"
WidgetEnd

Widget FPScribbleLower1
	FB_FP16DisplayLower "0"
WidgetEnd
```

**Note:** FB_FP8Display and FB_FP16Display are legacy feedback processors that now correspond to FB_FP8DisplayUpper and FB_FP16DisplayUpper respectively. The legacy versions will continue working for any .mst files where they already exist, but if you're creating a new set of files, you are encouraged to use the newer feedback processors.

