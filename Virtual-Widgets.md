If you read the [[Defining Control Surface Capabilities]] page, you'd know that we define Widgets in our .mst/.ost files to represent the capabilities of our surfaces. I sometimes think of these Widgets (or at least the Message Generator part) as triggers I can use to [[cause certain actions or behaviours|Defining Control Surface Behaviour]] to occur.

If we run with that analogy, then the Widgets on our surfaces are not the only things we may want to use to trigger behaviours. Sometimes we may want to trigger them when certain things occur in Reaper itself, eg when the selected track changes. 

These are called Virtual Widgets. 

We don't have to define them in our MST files, as they are not coming from a control surface. We just need to use them in our zone files like any other Widget.

The Virtual Widgets currently defined are:
* OnTrackSelection - fires when a track is selected in Reaper. 
* OnPageEnter - fires after a new Page has been entered.
* OnPageLeave - fires before the the old Page has been exited.
* OnInitialization -- fires when CSI initializes -- not valid for EuCon -- if needed for EuCon, will add later
