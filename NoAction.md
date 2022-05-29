The cunningly named NoAction action, does nothing. 
 
I'll just pause for a second while that sinks in. 

Now, contrary to what you might be thinking, this can be really useful. 

Let's say you [[GoZone|Zone-Actions]] from "Home" to "FX". As discussed on the [[Zones]] page, this effectively overlays "FX" over the top of "Home". 

Widgets mapped in "FX" take over "Home" behaviour.

Widgets mapped in "Home" but NOT in "FX" still work as they did before.

However, what if you want to stop this behaviour? Maybe in "FX", we want to cancel the "Home" behaviour to avoid operator errors. For example, perhaps in "Home" I have the transport controls mapped to widgets, but in"FX" I want to "disable" these mappings?

In that case, map them to NoAction in "FX" and they will defiantly ignore your presses until you [[GoZone|Zone-Actions]] back to "Home".