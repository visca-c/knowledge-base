## Adding custom editor setting toggle with Menu Item

#### My Use Case

I like to use Enable 'Enter Play Mode Options', it allows the disabling of 'Reload Domain', which speeds up the time it takes to enter play mode when building the game. It comes with a cost of having to manually handle some things (E.G static varaibles not resetting to default on their own), which could be a possible source for bug.

Sometimes a bug arise, and I suspect it was caused by 'Reload Domain' being disabled, I would want to quickly toggle 'Enter Play Mode Option' off, just to see if the bug persists. Soon enough, I realized that having to click through 'Project Settings/Editor' then scrolling down to find the 'Enter Play Mode Options' becomes tedious, so I looked for ways to make this process a little easier.

After some research, I found that the 'EditorSettings' class has a property 'enterPlayModeOptionsEnabled' I could use. Coupled with 'MenuItem' attribute, I was able to make a custom menu item that allows me to quickly toggle on/off 'Enter Play Mode Options'.

To make the menu item visually show the current state of the toggle, I used a menu item validate function, inside I call 'Menu.SetChecked' to show a check mark if 'Enter Play Mode Options' is enabled.

Now I can finally save 16 seconds of my life everytime I have to toggle this thing on/off (yes, I actually timed it).

![](Images/Menu%20Item%20Editor%20Setting%20(July%2015,%2024).jpg)