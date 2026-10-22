# Rainbow lightsaber

### @explicitHints false

## Introduction @showdialog

Your LED strip has **addressable LEDs**: every LED has a number, its address, and a tiny chip.
The Micro:bit sends the colours on one data wire, and each LED takes its own.

One strip has **10 LEDs**, numbered **0 to 9**. LED 0 is next to the wires.

![Wire it: red to 3V, yellow to pin 1, black to GND](https://www.startabit.com/projects/microbit-rainbow-lightsaber/images/2-wire-it.png)

## Step 1: Set up the strip and show a rainbow

Open ``||neopixel:Neopixel||``. Put ``||variables:set strip to||`` ``||neopixel:NeoPixel at pin P1 with 10 leds||``
into ``||basic:on start||``.

Then add ``||neopixel:set brightness||`` **40** and ``||neopixel:show rainbow from 1 to 360||``.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
strip.showRainbow(1, 360)
```

## Step 2: Download and look

Click **Download** to download the program to your Micro:bit, with the USB cable plugged in.

You should see a rainbow on the 10 LEDs. Nothing? Check the wires, and that the yellow wire is on **pin 1**.

## Step 3: Make the rainbow run

In ``||basic:forever||``: ``||neopixel:rotate pixels by 1||``, ``||neopixel:show||``, and ``||basic:pause||`` **50** ms.

**rotate** moves every colour one LED along. **show** sends the new colours to the strip.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
strip.showRainbow(1, 360)
basic.forever(function () {
    strip.rotate(1)
    strip.show()
    basic.pause(50)
})
```

## Step 4: A running light

Now one red light runs along the strip. Empty ``||basic:forever||`` and put a ``||loops:for index from 0 to 9||`` loop in it.

Inside the loop: ``||neopixel:clear||``, ``||neopixel:set pixel color at index to red||``, ``||neopixel:show||``
and ``||basic:pause||`` **30** ms.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
strip.showRainbow(1, 360)
basic.forever(function () {
    for (let index = 0; index <= 9; index++) {
        strip.clear()
        strip.setPixelColor(index, neopixel.colors(NeoPixelColors.Red))
        strip.show()
        basic.pause(30)
    }
})
```

## Step 5: Lightsaber, ignite with A

Time for the lightsaber. Drag the ``||basic:forever||`` block to the trash, and remove ``||neopixel:show rainbow||``.

In ``||input:on button A pressed||``: a ``||loops:for index from 0 to 9||`` loop that sets LED **index** to **blue**,
shows it and pauses **20** ms. The blade lights up from the handle to the tip.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
input.onButtonPressed(Button.A, function () {
    for (let index = 0; index <= 9; index++) {
        strip.setPixelColor(index, neopixel.colors(NeoPixelColors.Blue))
        strip.show()
        basic.pause(20)
    }
})
```

## Step 6: Retract with B

In ``||input:on button B pressed||``: the same loop, but set LED **9 − index** to **black**.
The blade goes off from the tip back to the handle.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
input.onButtonPressed(Button.A, function () {
    for (let index = 0; index <= 9; index++) {
        strip.setPixelColor(index, neopixel.colors(NeoPixelColors.Blue))
        strip.show()
        basic.pause(20)
    }
})
input.onButtonPressed(Button.B, function () {
    for (let index = 0; index <= 9; index++) {
        strip.setPixelColor(9 - index, neopixel.colors(NeoPixelColors.Black))
        strip.show()
        basic.pause(20)
    }
})
```

## Step 7: Test your lightsaber

**Download** again. Press **A** to ignite, **B** to retract.

Two strips plugged together? Change **10** to **20** and **9** to **19**.

## Step 8: Swing it: a white flash

The Micro:bit feels movement with its **accelerometer**.

In ``||input:on shake||``: ``||neopixel:show color||`` **white**, ``||basic:pause||`` **80** ms,
then ``||neopixel:show color||`` **blue** again.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
input.onGesture(Gesture.Shake, function () {
    strip.showColor(neopixel.colors(NeoPixelColors.White))
    basic.pause(80)
    strip.showColor(neopixel.colors(NeoPixelColors.Blue))
})
```

## Step 9: Tilt to change the colour

Make a variable **lit**: set it to **true** at the end of button A, and to **false** at the start of button B.

In ``||basic:forever||``: ``||logic:if||`` **lit**, set a variable **hue** to ``||math:map||``
``||input:acceleration (mg) x||`` from **-1023** to **1023** to **0** to **359**.
Then ``||neopixel:show color||`` with ``||neopixel:hue||`` **hue**, saturation **100**, luminosity **50**.

Tilting left and right turns the colour wheel. Now shake can end with **show color** of the same **hue**.

```blocks
let hue = 0
let lit = false
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
input.onButtonPressed(Button.A, function () {
    for (let index = 0; index <= 9; index++) {
        strip.setPixelColor(index, neopixel.colors(NeoPixelColors.Blue))
        strip.show()
        basic.pause(20)
    }
    lit = true
})
input.onButtonPressed(Button.B, function () {
    lit = false
    for (let index = 0; index <= 9; index++) {
        strip.setPixelColor(9 - index, neopixel.colors(NeoPixelColors.Black))
        strip.show()
        basic.pause(20)
    }
})
input.onGesture(Gesture.Shake, function () {
    if (lit) {
        strip.showColor(neopixel.colors(NeoPixelColors.White))
        basic.pause(80)
        strip.showColor(neopixel.hsl(hue, 100, 50))
    }
})
basic.forever(function () {
    if (lit) {
        hue = Math.map(input.acceleration(Dimension.X), -1023, 1023, 0, 359)
        strip.showColor(neopixel.hsl(hue, 100, 50))
    }
    basic.pause(50)
})
```

## Step 10: Swing your lightsaber

**Download** again. Press **A**, tilt the Micro:bit left and right, and give it a swing.

Want to swing it without the USB cable? Plug in the battery pack after the download.

## Well done! @showdialog

Your lightsaber ignites, retracts, flashes and changes colour.
More challenges are on the [project page](https://www.startabit.com/projects/microbit-rainbow-lightsaber/#challenges).

```package
neopixel=github:microsoft/pxt-neopixel
```
