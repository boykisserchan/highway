# Keyboard Layout Guide (more indepth :3)
By Shayaan Doodekula (@shayaan on Slack)

### Notes:
This guide preemptively assumes you are building a one piece, non-ergonomic keyboard. 
If you're NOT doing that, then you're on your own here pal (/j, there are other, better resources for you!)
It also assumes you're using Cherry MX Footprints, and if you're not, you're responsible for editing the settings to make sure the plugin works. 
It also assumes you have a *faint* idea of what you're doing. 

### Software used:
- KiCAD
- KiCAD "Keyboard footprints placer" plugin
- https://www.keyboard-layout-editor.com/


## Step 1: Layout Generation
Head over to https://www.keyboard-layout-editor.com/ to get started. 
Now, it's time to create your keyboard layout! 
You can use one of the presets (I used Default 60% for my keeb [W60](https://github.com/boykisserchan/W60)), or make your own layout (if you feel spicy :3)
You can use the Properties tab to set specific values.

![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/76ad556990e25cc334ac62d708ce6b4b21895f31_screenshot_2025-07-09_at_10.08.28___pm.png)

**Once you're done, download your layout using the Download JSON button.**

![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/58069f74e52842e95fdc32e0dc1d2daad8f3412d_screenshot_2025-07-09_at_10.14.20___pm.png)

## Step 2: Placement

### 2a. Schematic

Obviously, to create a keyboard PCB, you need to create a schematic. Creating a schematic is simple, but tedious at times.

First, create this piece in the KiCAD Schematic Editor. 
This is the base for each of your key. 
This consists of a keyswitch (SW_Push or SW_Push_45deg, I prefer 45deg) and a diode (D).
For ease of use in this guide, I'll be referring to this piece as a keyswitch collectively, even though it consists of a keyswitch and a diode.
![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/f9a47cc64666606e3f6231f0a3c6c17f0f008151_screenshot_2025-07-09_at_10.20.16___pm.png)

The exact shape of your schematic doesn't actually matter, and it won't change a thing, aside from wiring.
However, if you wanna make wiring look nicer (or rather, as nice as possible), then there's a method for that! 
If you don't care about that, then count the total amount of keys in your layout, and make a "square" (or close enough to) of keyswitches. 
Skip over the next chunk of text.

*If you want nicer wiring, and are willing to sacrifice pins for it then follow this method.*
First, count out the amount of keys in your first row. Place that many keys equally spaced (use the grid!) in a straight horizontal line. 
Repeat this for each row, ensuring the keys are both vertically and horizontally aligned with each other. 

Now, it's time to wire up your schematic. 
While you *could* wire the pins directly to the microcontroller, I like using Global Labels (cmd-L or ctrl-L) to make it look nicer. 
I name my labels Row # and Col #, but you could name them whatever you feel comfortable with, like the GPIO number. 
I highly suggest naming them after the Row and Col number though.
Once you're done, it should look a lil' like this:
![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/f514669531bf8a29f6cb90ca801d0d7231ffda06_screenshot_2025-07-09_at_10.43.26___pm.png)

Wire up the Row and Pin labels to your MC, and we can now do the grueling task of ✨footprint assignment✨
Go back to keyboard-layout-editor, and you'll see that the summary tab looks a little like this:
![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/4f3b12d1a8568502aaa40be800d3fdd4b1b7beb6_screenshot_2025-07-09_at_10.49.41___pm.png)

It'll be different for most people, but to break it down, here's what the numbers mean:

- **X.XX x 1**: This simply means a key with a width of X.XX unit and a height of 1 unit. The corresponding footprint is "Button_Switch_Keyboard:SW_Cherry_MX_X.XXu_PCB". (In the footprint, include trailing zeros.)
- **1 x 2**: This is a key with a width of 1 unit and a height of 2 units. The corresponding footprint is "Button_Switch_Keyboard:SW_Cherry_MX_2.00u_Vertical_PCB".
- **Literally anything else**: This is an abomination of God. (/j). This is likely a ISO Enter Key, but on the off chance it isn't, I'd try and find a footprint for the specific key you're using. I'd use the (ai03 plate generator)[https://kbplate.ai03.com/] to test if the ISO Enter Key footprint in KiCAD (Button_Switch_Keyboard:SW_Cherry_MX_ISOEnter_PCB) fits. I'm gonna ignore the existence of these, because it complicates things.

If you used my method that wastes pins, congrats! 
This makes your life easier to live!
I like having three desktops open: Your footprint assigner, your schematic, and Keyboard Layout Editor. 
Follow this three step process:

**Step 1**: Look at the key. Find it's width. Use the "Properties" tab.

**Step 2**: Find the key in the schematic. If you used my method, it's location will be correspondent to the row and column in Keyboard Layout Editor. If not, you'll need to go sequentally through both Keyboard Layout Editor and KiCAD. If you lose your spot, tough luck :( 

**Step 3**: Assign the key to the correct footprint. 

Once you've swiped more desktops than Swiper and have finished assigning your footprint, we can *finally* move on to the PCB!

**Finish up the rest of your schematic, and save your work!**

### 2b. PCB

Before opening the PCB Editor, let's install the "Keyboard footprints placer" plugin!
On the homepage, open the plugin manager!

![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/49ab78faf670c3e8dca566188f867d1b7d9a8c8c_screenshot_2025-07-09_at_10.12.33___pm.png)

Under "Plugins", search for "keyboard", and install this one:

![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/13d778f66f060a29ee9793d50d7a867fc6f54c12_screenshot_2025-07-09_at_11.33.19___pm.png)

Click "Install", and then "Apply Pending Changes". Close the tab, then open up your PCB Editor.

First, update the PCB from the schematic, and place your parts anywhere. The plugin will automatically move your keys and diodes, and you'll be in charge of where everything else goes.
Click the plugin's icon, it looks like a keyboard with a couple of blue keys.

![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/b46e7e3b0bb65fd3d57b8b94442ba95eb1e1d4ae_screenshot_2025-07-09_at_11.35.45___pm.png)

You'll get a GUI that looks like this:

![](https://hc-cdn.hel1.your-objectstorage.com/s/v3/ebb72dfd348dace9e3ec79deb9143a3532bccf51_screenshot_2025-07-09_at_11.47.11___pm.png)

You'll need to select the layout file (you downloaded this earlier!), and you're safe to continue after that. It'll automatically place your keys, and then you're done!

---
## Congrats, you just made your keyboard layout! Have fun placing the other components! DM me (@shayaan) on Slack if you have any questions!
