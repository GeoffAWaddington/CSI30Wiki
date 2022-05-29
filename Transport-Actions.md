The Transport Actions are fairly simple, and as a result, I often use them as "proof of life" actions when I'm first setting up a new surface mapping. If I can get a button on a surface mapped to the Play action, then I know I've got most of the basic setup for the surface working properly and I can move on to doing more advanced things. 

The Transport Actions are:
* Rewind
* FastForward
* Play
* Stop
* Record
* Loop
* Return to the beginning on the timeline

Little explanation needed for those, they map very directly to the buttons on Reaper's transport controls. 

They take no parameters, so if you want to map a Press widget to Play, you would put the following in your [[zone|Zones]] definition:

`PlayButton Play`

where PlayButton is the name of your widget. 
