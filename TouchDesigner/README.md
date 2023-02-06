## Touchdesigner mapping .tox for GoodPipes

This folder contains a .tox file which you can import into your TouchDesigner project, connect to an output TOP, and see the results displayed across the GoodPipes via Artnet.

### Instructions:

-   Connect your machine to the GoodPipes wifi network.
-   Import the .tox file into your TouchDesigner project, or open up the example .toe file (v2021.16)
-   If it itsn't already, connect a null DAT to the output of the .tox, that'll show you any artnet devices detected on the network. If you don't see any listed, check your wifi & firewall settings.
-   Connect a TOP with some image to the input of the .tox. You should see something on the LEDs.

### Parameters

-   `Master Level` float 0-1 controlling the master output level of the LEDs
-   `Pixels Per Bar` the number of pixels per bar (=the 'height' of the scaled image that gets mapped)
-   `NumBars` automatically creates `DMX OUT` CHOPS for each bar = 1 universe.
-   `Gamma` if you're matching your LEDs to a projector, you might want to adjust this so that the brightness curve more closely matches that of you projector / surface.
-   `MaxLevel` a weird parameter that should be experimented with for the content that you're displaying. Technically it should be set to 255, but in order to get a smooth fade up from 0 we have it set to 4.
-   `Input Smoothness` Switches between a smooth interpolated scale-down of the input TOP (`Interpolate Pixels`), or just grabbing the nearest pixel (`Nearest Pixel`) which will produce sharper lines.

### Things to check if it's not working, or looks wrong:

-   Open up the .tox, and look at the `DMX OUT` operators. There should be one per bar that you've specified using the `NumBars` parameter.
-   This is designed to work with RGB LED strips. If you're using RGBW, open up the .tox, find the `topto2` operator, and add `a` to the `Alpha` parameter. This should transmit a `white` signal to the appropriate LED segment.

### Caveats:

-   We're working on the mapping part of this right now, this is a first test to just get 'something' up on the LEDs, a more fully-featured version will follow.
-   This could be made more efficient and performant, so don't be surprised if adding this .tox drags your project frame rate down a little bit.
-   Because we were running this over wifi, in order to keep the data rate sensible it's currently set to update at 30fps, which could be increased depending on your network speed. To update this, change the `Rate` parameter on `dmxout3`, and click `Recreate All Operators` on the `Replicator` COMP.
