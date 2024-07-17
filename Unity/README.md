## Speed Up Setting Toggle With Custom Menu Item

#### Overview

I like to use Enable '**Enter Play Mode Options**', it allows the disabling of '**Reload Domain**', which speeds up the time it takes to enter play mode when developing the game. This setting speeds up the process but comes with the cost of not resetting static variables by default, which sometimes introduces bugs.

#### The Challenge

I frequently encountered bugs that I suspected were related to 'Reload Domain' being disabled. To quickly verify the cause, I needed an efficient way to toggle 'Enter Play Mode Options'. However, navigating through 'Project Settings' to 'Editor' and then scrolling down to find the 'Enter Play Mode Options' was becoming tedious, so I looked for ways to make this process a little easier.

#### The Solution

After some research, I found that the '**EditorSettings**' class has a '**enterPlayModeOptionsEnabled**' property. Coupled with '**MenuItem**' attribute, I was able to make a custom menu item that allows me to quickly toggle on/off 'Enter Play Mode Options'.

##### Adding a Validation Function

To make the menu item visually show the current state of the toggle, I used a menu item validate function, inside I call '**Menu.SetChecked**' to show a check mark if 'Enter Play Mode Options' is enabled.

Now I can finally save 16 seconds of my life everytime I have to toggle this thing on/off (yes, I actually timed it).

![](Images/Menu%20Item%20Editor%20Setting%20(July%2015,%2024).jpg)