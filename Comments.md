You can add comments to your .mst or .zon files to either leave notes for yourself or other users of your files, or even comment out lines of code you don't want to use.

## Comments
If you want to comment out an entire line of code the first character of that row of text must be a slash /

```` 
/ Here's how I'd comment out a line of a text to add comments in an an .mst or .zon file.
```` 

Maybe you'd use this for instructions, or to help break out sections of an .mst file, or to tell CSI to ignore a particular row in a .zon file to comment that section of code out entirely.

## Trailing Comments
Very frequently, you may want to add trailing comments to your .zon file in order to indicate what a particular CSI or Reaper action does. Trailing comments can be after the .zon instructions, and must begin with two slash characters //

In this example, I've labeled the Track Automation modes so I can make sure the modes are assigned to the correct buttons.
```` 
Zone "Buttons|"
	Trim 				TrackAutoMode 0 	//Trim Read
	Read 				TrackAutoMode 1 	//Read
	Touch 				TrackAutoMode 2 	//Touch
	Write 				TrackAutoMode 3 	//Write
	Latch				TrackAutoMode 4 	//Latch
        Drop                            TrackAutoMode 5 	//Latch Preview
ZoneEnd
```` 

Anything after the two slashes is ignored, so this becomes a very handy way to keep track of what's happening in your .zon files.
