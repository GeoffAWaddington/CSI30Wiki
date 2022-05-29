The NoFeedback action is meant to turn off feedback to a specific widget. This is especially useful if you have a control surface that is designed to show feedback on buttons, but those buttons are assigned to Reaper actions where feedback doesn't make sense because there is no on or off state. 

In the below example, the Marker button is being used to insert a Marker at the current or edit position. On some control surfaces, this may cause the marker button to light up, which doesn't make sense. So to solve for this in our zone file, we need to use the NoFeedback action as shown below:

```
Zone "Buttons"
     Marker                   Reaper 40171     // Insert marker at current or edit position
     Property+Marker          NoFeedback       // Turns off feedback
ZoneEnd    
```

Here you see that the **Property+** modifier is combined with the widget name from the row **immediately above** followed by the NoFeedback action. If the **Property+** line doesn't immediately follow the widget+action row it's meant to apply to, this will not work.

This very same approach also works with modifiers as shown below:

```
Zone "Buttons"
     Marker                     Reaper 40171     // Insert marker at current or edit position
     Property+Marker            NoFeedback       // Turns off feedback
     Option+Marker              Reaper 40173     // Go to next marker or project end
     Property+Option+Marker     NoFeedback       // Turns off feedback	
     Shift+Marker               Reaper 40172     // Go to previous marker or project start
     Property+Shift+Marker      NoFeedback       // Turns off feedback
ZoneEnd
```
