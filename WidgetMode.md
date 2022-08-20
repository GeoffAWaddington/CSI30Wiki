## WidgetMode
WidgetMode currently has no functionality (exists only in the CSI Exp builds as of August 19, 2022) but is intended to allow CSI to have greater control of device specific features, such as displays. This page will be updated when device-specific functionality gets added.

One proposed example: what's currently accomplished by all these Property+ values below...
```
LEDRingC| TrackSendVolume
Property+LEDRingC| Ringstyle 01
Property+LEDRingC| RingColor 00 255 00 255
Property+LEDRingC| RingColor 01 00 255 00
Property+LEDRingC| RingColor 75 255 255 00
Property+LEDRingC| RingColor 90 255 00 00
```

...may eventually be able to be replaced with just this...
```
LEDRingC| TrackSendVolume
LEDRingC| WidgetMode "Ringstyle 01 RingColors 00 255 00 255   01 00 255 00   75 255 255 00   90 255 00 00"
```