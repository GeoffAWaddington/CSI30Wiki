In CSI, a Page is the highest level of the hierarchy - with each Page containing one or more surfaces, and each surface having an .mst (if MIDI/MCU) or .ost (if OSC) file which describes the surface properties as well as a zone folder defining what exactly that surface will do in Reaper. CSI requires that at least one page exist (called "HomePage" by default) before surfaces can be added. 

But CSI allows you to create multiple Pages, with each Page containing the configuration information for whatever Surfaces and Actions you want CSI to recognize. Only one Page can be active in Reaper at any time, but you can switch between Pages easily and instantaneously similar to activating a zone. 

Pages allow you to have....

* Different surfaces enabled/disabled depending on what you're doing - i.e. different surfaces enabled on each Page
* Different actions assigned to your surface depending on what your doing - i.e. different zone folders for one or more surfaces on each Page
* Surface capabilities defined differently on each page - i.e. different .mst and/or files for one or more on each Page

## **To create a Page**

1. Open Reaper's Preferences -> Control/OSC/Web
2. Select CSI
3. Click Edit to open the CSI Settings
4. On the middle-section of the CSI Settings, you can add a Page by clicking the Add button
5. Highlight the name of the Page you want to edit from the left, and add or modify surfaces on the right-hand portion of the screen

![CSI Settings Add Page Image](https://i.imgur.com/02MHtzu.png)

CSI's included support files should already have a page called "HomePage" created for you to make first-time setup easy. If not there, as a best practice, your first page should be called "HomePage" (if anything just for consistency). Any pages you create beyond that can be named whatever you'd like.

Now you can begin adding surfaces (Add MIDI or Add OSC buttons) and selecting which .mst/.osc and Zone folder each Page will utilize. Remember: you can use completely different .mst files and/or Zone folders for each Page, completely changing how each surface will work on each Page. 

See [[Installation|Installation-and-Setup]] for instructions on how to setup a surface.

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

Finally, the PageNameDisplay action can be assigned to a Display widget in order to show the name of the currently active Page.

```
MainDisplay     PageNameDisplay
```