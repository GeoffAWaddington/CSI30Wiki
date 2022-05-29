Use the FixedTextDisplay action when you want to show static text within one of your displays. 

In this example, I want to see the words "Pre/Post" in my display:
```
	DisplayUpperA1 FixedTextDisplay "Pre/Post"
```

Tip: if you're in a zone with navigator where you're controlling multiple displays, you can use the pipe character | in your fixed text display to show the number. 

For instance, this in your zone...
```
Zone "Send"
     SendNavigator
	DisplayUpperD|		FixedTextDisplay "Send| Pan"
ZoneEnd
```

Would appear on your display on your surface as: 
```
DIsplayUpperD1 "Send1 Pan"
DisplayUpperD2 "Send2 Pan"
DisplayUpperD3 "Send3 Pan"
etc.
````