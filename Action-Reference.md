## CSI Actions

### Transport and Timeline
* [[Rewind|Transport Actions]]
* [[FastForward|Transport Actions]]
* [[Play|Transport Actions]]
* [[Stop|Transport Actions]]
* [[Record|Transport Actions]]
* [[CycleTimeline|Transport Actions]]
* [[MCUTimeDisplay|Transport Actions]]
* [[CycleTimeDisplayModes|TimeDisplay#cycletimedisplaymodes]]

### Tracks
* [[TrackVolume]]
* [[SoftTakeover7BitTrackVolume]]
* [[SoftTakeover14BitTrackVolume]]
* [[MCUTrackPan]]
* [[TrackPan]]
* [[TrackPanWidth]]
* [[TrackPanL]]
* [[TrackPanR]]
* [[TrackSelect]]
* [[TrackUniqueSelect|TrackSelect]]
* [[TrackRangeSelect|TrackSelect]]
* [[TrackSolo]]
* [[TrackMute]]
* [[TrackInvertPolarity]]
* [[TrackRecordArm]]
* [[TrackNameDisplay]]
* [[TrackVolumeDisplay]]
* [[MCUTrackPanDisplay|MCUTrackPan]]
* [[TrackPanDisplay]]
* [[TrackPanWidthDisplay]]
* [[TrackPanLeftDisplay]]
* [[TrackPanRightDisplay]]
* [[TrackOutputMeter]]
* [[TrackOutputMeterAverageLR|TrackOutputMeter#TrackOutputMeterAverageLR]]
* [[TrackOutputMeterMaxPeakLR|TrackOutputMeter#TrackOutputMeterMaxPeakLR]]

### Track Sends
* [[TrackSendVolume|Send-Zones#send-actions]]
* [[TrackSendPan|Send-Zones#send-actions]]
* [[TrackSendMute|Send-Zones#send-actions]]
* [[TrackSendPrePost|Send-Zones#send-actions]]
* [[TrackSendStereoMonoToggle|Send-Zones#send-actions]]
* [[TrackSendInvertPolarity|Send-Zones#send-actions]]
* [[TrackSendNameDisplay|Send-Zones#send-actions]]
* [[TrackSendVolumeDisplay|Send-Zones#send-actions]]
* [[TrackSendPanDisplay|Send-Zones#send-actions]]
* [[TrackSendPrePostDisplay|Send-Zones#send-actions]]

### Track Receives
* [[TrackReceiveVolume|Receive-Zones#receive-actions]]
* [[TrackReceivePan|Receive-Zones#receive-actions]]
* [[TrackReceiveMute|Receive-Zones#receive-actions]]
* [[TrackReceivePrePost|Receive-Zones#receive-actions]] 
* [[TrackReceiveInvertPolarity|Receive-Zones#receive-actions]] 
* [[TrackReceiveNameDisplay|Receive-Zones#receive-actions]] 
* [[TrackReceiveVolumeDisplay|Receive-Zones#receive-actions]]
* [[TrackReceivePanDisplay|Receive-Zones#receive-actions]]
* [[TrackReceivePrePostDisplay|Receive-Zones#receive-actions]]

### FX
* [[FXParam|FX Parameter Mapping Actions#FXParam]] 
* [[FXNameDisplay|FX Parameter Mapping Actions#FXParamValueDisplay]] 
* [[FXParamNameDisplay|FX Parameter Mapping Actions#FXParamNameDisplay]] 
* [[FXParamValueDisplay|FX Parameter Mapping Actions#FXParamValueDisplay]] 
* [[FXMenuNameDisplay|FX-Parameter-Mapping-Actions#fxmenunamedisplay]]
* [[FocusedFXParam|FX Parameter Mapping Actions#focusedfxparam]]
* [[FocusedFXParamNameDisplay|FX Parameter Mapping Actions#focusedfxparamnamedisplay-and-focusedfxparamvaluedisplay]]
* [[FocusedFXParamValueDisplay|FX Parameter Mapping Actions#focusedfxparamnamedisplay-and-focusedfxparamvaluedisplay]]
* [[ToggleEnableFocusedFXMapping|FX Parameter Mapping Actions#ToggleEnableFocusedFXMapping]]
* [[ToggleEnableFocusedFXParamMapping|FX Parameter Mapping Actions#ToggleEnableFocusedFXParamMapping]]
* [[ToggleFXBypass|FX Parameter Mapping Actions#ToggleFXBypass]]
* [[FXBypassedDisplay|FX Parameter Mapping Actions#FXBypassedDisplay]]
* [[FXGainReductionMeter|FX Parameter Mapping Actions#FXGainReductionMeter]]
* [[GoFXSlot|FX-Parameter-Mapping-Actions#gofxslot]]

### Navigation
* [[TrackBank]]
* [[SelectedTrackBank]]
* [[TrackSendBank]]
* [[TrackReceiveBank]]
* [[TrackFXMenuBank]]
* [[SelectedTrackSendBank]]
* [[SelectedTrackReceiveBank]]
* [[SelectedTrackFXMenuBank]]
* [[ToggleScrollLink]]
* [[ForceScrollLink]]
* [[NextPage|Pages#paging-actions]]
* [[GoPage|Pages#paging-actions]]
* [[GoHome]]
* [[GoSubZone|FX-Sub-Zones]]
* [[LeaveSubZone]]
* [[PageNameDisplay|Pages#pagenamedisplay]]
* [[GoTrackSend]]
* [[GoTrackReceive]]
* [[GoTrackFXMenu]]
* [[GoSelectedTrackSend]]
* [[GoSelectedTrackReceive]]
* [[GoSelectedTrackFXMenu]]
* [[GoSelectedTrackFX]]

### Project Actions
* [[SaveProject]]
* [[Undo]]
* [[Redo]]

### VCA
* [[TrackToggleVCASpill|VCA's-and-VCA-Spill#togglevcamode]]
* [[CycleTrackVCAFolderModes]]
* [[TrackVCAFolderModeDisplay]]

### Automation
* [[TrackAutoMode|Automation-Actions#trackautomode]]
* [[GlobalAutoMode|Automation-Actions#globalautomode]]
* [[CycleTrackAutoMode|Automation-Actions#cycletrackautomode]]
* [[TrackAutoModeDisplay|Automation-Actions#trackautomodedisplay]]

### Other
* [[NoAction]]
* [[FixedTextDisplay]]
* [[FixedRGBColourDisplay]]
* [[ClearAllSolo]]
* [[NoFeedback]]
* [[Broadcast]]
* [[Receive]]

### Modifiers
* [[Shift|Modifiers#global-modifiers]]
* [[Option|Modifiers#global-modifiers]]
* [[Control|Modifiers#global-modifiers]]
* [[Alt|Modifiers#global-modifiers]]
* [[Touch|Modifiers#touch]]
* [[InvertFB|Modifiers#invertfb]]
* [[Hold|Modifiers#hold]]
* [[Flip]]

### Reaper Actions
```    
Button1 Reaper "40454"    
Button2 Reaper "_0e5b196e7f67994bab6de09c49f05926"    
Button3 Reaper "_SWSTL_SHOWALL"    
```
Invokes the Reaper Action (custom or built in) specified by the ID in the argument. The syntax is the word Reaper followed by the Command ID. How do you get the Command ID? Open the Reaper Action List, right-click on the action name, and select "Copy selected action command ID" which copies it to your clipboard. Paste the Command ID into the .zon file at the appropriate location. 

As a best practice, you can label the command ID in your .zon using two forward slashes to leave trailing comments like this:
```    
Button1 "_S&M_FXBYPLAST"     // Bypasses last touched FX on the selected track
```


