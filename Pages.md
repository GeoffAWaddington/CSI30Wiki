In CSI, a Page is the highest level of the hierarchy - with each Page containing one or more surfaces, and each surface having an .mst (if MIDI/MCU) or .ost (if OSC) file which describes the surface properties as well as a zone folder defining what exactly that surface will do in Reaper. CSI requires that at least one page exist (called "Home" by default) before surfaces can be added. 

But CSI allows you to create multiple Pages, with each Page containing the configuration information for whatever Surfaces and Actions you want CSI to recognize. Only one Page can be active in Reaper at any time, but you can switch between Pages easily and instantaneously similar to activating a zone. 

Pages allow you to have....

* Different surfaces enabled/disabled depending on what you're doing - i.e. different surfaces enabled on each Page
* Different actions assigned to your surface depending on what your doing - i.e. different zone folders for one or more surfaces on each Page
* Surface capabilities defined differently on each page - i.e. different .mst and/or files for one or more on each Page

## **To create a Page:**

1. Open Reaper's Preferences -> Control/OSC/Web
2. Select CSI
3. Click Edit to open the CSI Settings
4. On the left side of the CSI Settings, you can add a Page by clicking the Add button
5. Highlight the name of the Page you want to edit from the left, and add or modify surfaces on the right-hand portion of the screen

![CSI Settings Add Page Image](https://i.imgur.com/GtvtjFF.png)

As a best practice, your first page should be called "Home" (if anything just for consistency). Any pages you create beyond that can be named whatever you'd like.

Now you can begin adding surfaces (Add MIDI or Add OSC buttons) and selecting which .mst/.osc and Zone folder each Page will utilize. See [Installation](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/Installation) for instructions on how to setup a surface.

## **Page Settings**
If you select a Page and click the **Edit **button, there are some options for...

![CSI Page Settings Image](https://i.imgur.com/sdKCT8V.png)

**Track visibility on surface follows**: select Mixer Control Panel to have the surface visibility follow Reaper's MCP or Track Control Panel to have it follow the TCP. Example: Reaper allows you to hide a track in Reaper's Mixer (MCP) but still keep it visible in the TCP. Would you want to see that track on your control surface? If no, because it's hidden in the mixer and therefore should be on the surface, set track visibility on the surface to follow the Mixer Control Panel. If you'd prefer, you can have surface visibility follow the TCP view. 

**Banks with other checked Pages**: if checked, this will ensure your Pages will stay in 'sync'. So if you're banked to a particular place in the mixer and then change Pages, you stay in the same place.

**Reaper GUI follows surface**: if checked, the on screen Mixer/Arrange window will bank with the surfaces. Can also be enabled/disabled with the [ToggleScrollLink](https://github.com/GeoffAWaddington/reaper_csurf_integrator/wiki/ToggleScrollLink) action.

## Paging Actions
If using multiple Pages, the buttons on surface can be assigned to switch between Pages. Use the CSI **GoPage** action to jump between Pages in CSI. Use **NextPage** to cycle between Pages. In the below example, I have a Page called Home and another called Mix...

On my Home page, I may want to utilize the "Channel" button on my surface to enter the Mix Page.
````
Zone "Buttons"
        Channel         GoPage "Mix"  // Activates the Mix page
ZoneEnd
````

But in order to get back to the Home page, I probably want to make sure I have the opposite happening when the Mix page is active.
````
Zone "Buttons"
        Channel         GoPage "Home"  // Activates the Home page
ZoneEnd
````

You could also use the NextPage action to just cycle through the Pages in your CSI setup. In a two-page setup this would essentially be a toggle but if you had 3 or more pages it will cycle through them.
```
Zone "Buttons"
        Channel         NextPage  // Cycles through the list of pages
ZoneEnd
```

Finally, while not a Navigation Action, the PageNameDisplay action can be assigned to a Display widget in order to show the name of the currently active Page.

```
MainDisplay     PageNameDisplay
```