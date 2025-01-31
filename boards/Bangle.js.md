<!--- Copyright (c) 2019 Gordon Williams, Pur3 Ltd. See the file LICENSE for copying permission. -->
Bangle.js
==========

<span style="color:red">:warning: **Please view the correctly rendered version of this page at https://www.espruino.com/Bangle.js. Links, lists, videos, search, and other features will not work correctly when viewed on GitHub** :warning:</span>

* KEYWORDS: Espruino,Official Board,nRF52832,nRF52,Nordic,Board,PCB,Pinout,Bluetooth,BLE,Bluetooth LE,Graphics,Bangle.js,Bangle,Banglejs,Smartwatch,Watch

[![](Banglejs/bangle-leaf.jpg)](https://www.kickstarter.com/projects/gfw/banglejs-the-hackable-smart-watch)

[![NOW FUNDING ON KICKSTARTER!](Banglejs/ks.png)](https://www.kickstarter.com/projects/gfw/banglejs-the-hackable-smart-watch)

* BUYFROM: £47,,https://www.kickstarter.com/projects/gfw/banglejs-the-hackable-smart-watch

**Bangle.js is an open, hackable smartwatch**

You can easily install new apps from the web or develop your own using JavaScript or a graphical programming language (Blockly). All you need is a Web Browser (Chrome, Edge or Opera) and you can upload apps or write code to run on your watch wirelessly! Bangle.js is waterproof and AI enabled and comes with bluetooth low energy, GPS, a heart rate monitor, accelerometer and more.

Contents
--------

* APPEND_TOC

Features
--------

* IP68 Waterproof: up to 10m underwater
* Nordic 64MHz nRF52832 ARM Cortex-M4 processor with Bluetooth LE
* 64kB RAM 512kB on-chip flash, 4MB external flash
* 1.3 inch 240x240 16 bit LCD display with 2 zone touch
* GPS/Glonass receiver (UBlox)
* Heart rate monitor
* 3 Axis Accelerometer (with Pedometer and Tap detect)
* 3 Axis Magnetometer
* Piezo speaker and Vibration motor
* 350mAh battery, 1 week standby time
* 5 x 5 x 1.7 cm case, plastic with stainless steel ring
* Can be disassembled with just 4 screws


Power Consumption
-----------------

* Idle, accelerometer on - 0.7mA
* BLE Connected in high bandwidth mode - 0.5mA
* Compass on - 2mA
* Heart rate monitor on - 2.5mA
* 100% CPU usage running JavaScript - 7mA
* GPS on - 30mA
* LCD on - 40mA

This means that when idle (in the normal power-on state) you can expect around 20 days of battery life.


Powering off if completely broken
-----------------------------------

* Long-press `BTN1` + `BTN2` for about 6 seconds until the screen goes blank
* Keep pressing them while `====` goes across the screen
* Watch will start vibrating
* Release both buttons
* Your watch may restart if it hasn’t been turned off since the last firmware update. If so, repeat the process again.


Resetting
----------

* Long-press `BTN1` + `BTN2` for about 6 seconds until the screen goes blank
* Release both buttons
* Bangle.js will boot as if it just turned on normally (although the current time will be lost)

If you release the buttons too late you'll enter bootloader mode, in
which case you need to press `BTN1` to exit.


Resetting without loading any code
-----------------------------------

If you uploaded some code that runs at startup and breaks Bangle.js you may need to do this.

It won’t delete anything, so unless you fix/remove the broken code (see "Deleting all Code") Bangle.js will remain broken next time it restarts.

* Long-press `BTN1` + `BTN2` for about 6 seconds until the screen goes blank
* Release `BTN2` but keep pressing `BTN1` while `====` goes across the screen
* Keep holding `BTN1` while Bangle.js boots
* Release it - you should have the Bangle.js logo, version, and MAC address


Deleting all code
-----------------

You can do this either while your watch is connectable, or
if you have reset it without loading any code (above).

* Go to https://banglejs.com/apps
* Go to `About -> Remove All Apps`
* Re-install `Bootloader`, a `Clock`, and `Settings` if you want it


Deleting apps
-------------

* If you can access the menus on your device and the `App Manager` app is installed, you can delete apps using the `App Manager`
* You can go to https://banglejs.com/apps and click `Connect`. Under `My Apps` your installed apps are listed, and you can click the 'Bin' icon next to them to remove them
* If you hit any issues with installed apps and can't access the menus on your device, then follow the instructions above for "Resetting without loading any code" above.


Tutorials
--------

First, it's best to check out the [Getting Started Guide](/Quick+Start+BLE#banglejs)

There is more information below about using the [LCD](#lcd) and [onboard peripherals](#onboard) as well.

Tutorials using Bangle.js:

* APPEND_USES: Bangle.js

Tutorials using Bluetooth LE:

* APPEND_USES: Only BLE,-Bangle.js

Tutorials using Bluetooth LE and functionality that may not be part of Bangle.js:

* APPEND_USES: BLE,-Only BLE,-Bangle.js

There are [many more tutorials](/Tutorials) that may not be specifically for
you device but will probably work with some tweaking. [Try searching](/Search)
to find what you want.

Information
-----------

... to be added soon ...


<a name="lcd"></a>LCD Screen
---------------------------------

Bangle.js displays the REPL (JavaScript console) by
default, so any calls like `print("Hello")` or `console.log("World")` will output
to the LCD when there is no computer connected via Bluetooth or [Serial](#serial-console).
Any errors generated when there is no connection will also be displayed on the LCD.

### Graphics

You can output graphics on Bangle.js's display via the global variable `g`
that is an instance of the [Graphics class](/Reference#Graphics). By default the display
is unbuffered, so any changes will take effect immediately.

```
// Draw a pattern with lines
g.clear();
for (i=0;i<64;i+=7.9) g.drawLine(0,i,i,63);
g.drawString("Hello World",30,30);
```

### Screen buffering

Bangle'js's screen is 240 x 240 x 16 bits - which uses substantially more
memory than the microcontroller has RAM. As such, draw commands go straight
to the screen (and `g.getPixel` will not work).

Drawing straight to the screen can cause flicker, so for games we'd recommend
that you use `Bangle.setLCDMode("doublebuffered")`. When in this mode the
LCD resolution is lowered to 240 x 160 pixels and draw commands will not
be visible until you call `g.flip()`.

You can call `Bangle.setLCDMode()` to return to normal, unbuffered mode.

### Menus

Bangle.js comes with a built-in menu library that can be accessed with the [`E.showMenu()`](/Reference#l_E_showMenu) command.

```
// Two variables to update
var boolean = false;
var number = 50;
// First menu
var mainmenu = {
  "" : {
    "title" : "-- Main Menu --"
  },
  "Backlight On" : function() { LED1.set(); },
  "Backlight Off" : function() { LED1.reset(); },
  "Submenu" : function() { E.showMenu(submenu); },
  "A Boolean" : {
    value : boolean,
    format : v => v?"On":"Off",
    onchange : v => { boolean=v; }
  },
  "A Number" : {
    value : number,
    min:0,max:100,step:10,
    onchange : v => { number=v; }
  },
  "Exit" : function() { E.showMenu(); },
};
// Submenu
var submenu = {
  "" : {
    "title" : "-- SubMenu --"
  },
  "One" : undefined, // do nothing
  "Two" : undefined, // do nothing
  "< Back" : function() { E.showMenu(mainmenu); },
};
// Actually display the menu
E.showMenu(mainmenu);
```

See http://www.espruino.com/graphical_menu for more detailed information.

### Terminal

Bangle.js's LCD acts as a VT100 Terminal. To write text to the LCD regardless of
connection state you can use `Terminal.println("your text")`. Scrolling
and simple VT100 control characters will be honoured.

You can even move the JavaScript console (REPL) to the LCD while connected
via Bluetooth, and use your bluetooth connection as a simple keyboard using
the following commands:

```
Bluetooth.on("data",d=>Terminal.inject(d));
Terminal.setConsole();
```

<a name="onboard"></a>On-device Peripherals
------------------------------------------------------

Most peripherals on the device are accessible via fields
and events on the [Bangle](https://banglejs.com/reference#t_Bangle) object.

### LED

There are no LEDs on Bangle.js - so there are no built-in `LED` variables.

### Buttons

There are 5 buttons on Bangle.js. The 3 physical buttons on the right are (top to bottom) `BTN1`, `BTN2` and `BTN3`, and the screen has two touch areas, on the left (`BTN4`) and right (`BTN5`).

* `BTN1` - ‘Up/Previous’ in menus
* `BTN2` - ‘Select’ in menus, or bring up menu when in Clock
* `BTN3` - Down/Next in menus
* `BTN4` - Left-hand side of touchscreen. Used for some games, but not in menus
* `BTN5` - Right-hand side of touchscreen. Used for some games, but not in menus


* You can access a button's state with `digitalRead(BTN1)` or `BTN1.read()`
(the two commands are identical). `BTN` is also defined, and is the same as `BTN1`.
* Polling to get the button state wastes power, so it's better to use `setWatch`
to call a function whenever the button changes state:

```
setWatch(function() {
  console.log("Pressed");
}, BTN, {edge:"rising", debounce:50, repeat:true});
```


### Accelerometer

The accelerometer runs all the time and produces `accel` events on the
`Bangle` object.

```
Bangle.on('accel', function(acc) {
  // acc = {x,y,z,diff,mag}
});
```

See [the reference](https://banglejs.com/reference#t_l_Bangle_accel) for
more information.

#### Gestures

When a sudden movement is detected, the accelerations in it are recorded
and a [`gesture` event](https://banglejs.com/reference#l_Bangle_gesture)
is created.

If `.tfmodel` and `.tfnames` files are created in storage, Tensorflow
AI will be run on the model with the gesture information and an
[`aiGesture`](https://banglejs.com/reference#l_Bangle_aiGesture) event
will be created with the name of the detected gesture.

### Compass

The compass can be turned on with `Bangle.setCompassPower(1)` and when
enabled, `mag` events are created 12.5 times a second:

```
Bangle.setCompassPower(1)
Bangle.on('mag', function(mag) {
  // mag = {x,y,z,dx,dy,dz,heading}
});
```

See [the reference](https://banglejs.com/reference#t_l_Bangle_mag) for
more information.

### GPS

The GPS can be turned on with `Bangle.setGPSPower(1)` and when
enabled, `GPS` events are created once a second:

```
Bangle.setGPSPower(1)
Bangle.on('GPS', function(gps) {
  // gps = {lat,lon,alt,speed,etc}
});
```

`GPS-raw` events are also created containing a String for each
NMEA line that comes from the GPS receiver. These contain far more
detailed information from the GPS.

See [the reference](https://banglejs.com/reference#l_Bangle_GPS) for
more information.

### Heart rate

A proper API will be added for this before release with heart rate detection,
but for now:

```
Bangle.ioWr(0x80,0); // turn HRM on

var a = analogRead(D29); // read the raw PPG value
```


Firmware Updates
------------------

  * Long-press `BTN1` + `BTN2` for about 6 seconds until the screen goes blank
  * Release `BTN2`
  * Release `BTN1` a moment later while `====` is going across the screen
  * The watch should now be in Dfu mode
* Install the Nordic Semiconductor NRF Toolbox App for Android or iOS
* Download the latest stable distribution zip from the [Espruino site](https://www.espruino.com/Download#banglejs) or the latest bleeding edge nightly build from [here](http://www.espruino.com/binaries/travis/master/).
* Open the NRF Toolbox app
* Tap the DFU icon
* Tap Select File, choose Distribution Packet (ZIP), and choose the ZIP file you downloaded
* If a Select scope window appears, choose All
* Tap Select Device and choose the device called `DfuTarg`
* Now tap Upload and wait
* It will take around 90 seconds to complete
* Once complete, long-press `BTN3` to go to the Clock
* You should still have the original apps you installed


Troubleshooting
---------------

For more answers please check out the [Bluetooth Troubleshooting](Troubleshooting+BLE) or [General Troubleshooting](/Troubleshooting) pages.



Other Official Espruino Boards
------------------------------

* APPEND_KEYWORD: Official Board
