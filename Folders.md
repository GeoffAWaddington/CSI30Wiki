If you prefer to work with folders within Reaper, it's easy to create a [VCA](VCA's-and-VCA-Spill)-like workflow by combining CSI with some existing Reaper actions!

You'll be able to expand and collapse folders directly from your surface and toggle between seeing all tracks and a folder only view.

## Expanding and Collapsing Folders
Let's say your mix makes extensive use of folder tracks and you'd like the option to quickly expand and collapse your folders right from your surface. You can do that with a combination of CSI plus some carefully sequenced Reaper actions.

In the below example, I'm using Option+Select to expand or collapse the children of the selected folder track, essentially being able to expand or collapse a folder. You'll notice that we have two different actions being triggered from a single widget. CSI will run these two actions in sequence from a single button press (in this case, a modified button press).
```` 
Zone "Track"
    Option+Select|   TrackUniqueSelect
    Option+Select|   Reaper 41665       //Mixer: Show/hide children of selected tracks
ZoneEnd
```` 

## Toggle "Folder Mode"
Similarly, if you would like to create a Mixer view that will toggle back and forth between displaying all tracks on your surface, or only showing folder tracks, CSI also allows you to do that. 

In the below example, we're able to create a simple macro that selects all top-level folder tracks and toggles between showing and hiding the children in the mixer. CSI will run these three actions in order upon the firing of this action.
```` 
Zone "Buttons"
        Shift+F4     Reaper 41803     //Track: Select all top level tracks
        Shift+F4     Reaper 41665     //Mixer: Show/hide children of selected tracks
        Shift+F4     Reaper 40297     //Track: Unselect all tracks
ZoneEnd
```` 

Note: these actions will also impact track visibility in the MCP.

