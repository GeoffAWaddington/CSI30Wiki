Use this widget for Qcon Pro X|iCON master meters. 

If you have one of these surfaces, your widgets should look like this in the .mst:

```` 
Widget MasterChannelMeterLeft
	FB_QConProXMasterVUMeter
WidgetEnd

Widget MasterChannelMeterRight
	FB_QConProXMasterVUMeter
WidgetEnd
```` 

Also, make sure you specify Left (0) and Right (1) in the.zon file.

```` 
Zone "MasterTrack"
MasterTRackNavigator
    MasterChannelMeterLeft TrackOutputMeter  0
    MasterChannelMeterRight TrackOutputMeter  1
ZoneEnd
```` 