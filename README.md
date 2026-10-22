# Rainbow lightsaber

### @explicitHints false

## Introduction @showdialog

Your LED strip has **addressable LEDs**: every LED has a number, its address, and a tiny chip.
The Micro:bit sends the colours on one data wire, and each LED takes its own.

One strip has **10 LEDs**, numbered **0 to 9**. LED 0 is next to the wires.

![Wire it: red to 3V, yellow to pin 1, black to GND](https://www.startabit.com/projects/microbit-rainbow-lightsaber/images/2-wire-it.png)

## Step 1: Set up the strip

In ``||basic:on start||``: ``||variables:set strip to||`` ``||neopixel:NeoPixel at pin P1 with 10 leds||``,
then ``||neopixel:set brightness||`` **40** and ``||neopixel:show rainbow from 1 to 360||``.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
strip.showRainbow(1, 360)
```

## Step 2: Download and look

**Download** with the USB cable plugged in: a rainbow on the 10 LEDs.
Nothing? Check that the yellow wire is on **pin 1**.

## Step 3: Make the rainbow run

In ``||basic:forever||``: ``||neopixel:rotate pixels by 1||``, ``||neopixel:show||``, ``||basic:pause||`` **50** ms.

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

Empty ``||basic:forever||``. Put ``||loops:for index from 0 to 9||`` in it, and inside:
``||neopixel:clear||``, set pixel **index** to **red**, ``||neopixel:show||``, pause **30** ms.

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

Delete ``||basic:forever||`` and ``||neopixel:show rainbow||``. In ``||input:on button A pressed||``:
for index 0 to 9, set pixel **index** to **blue**, show, pause **20** ms.

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

In ``||input:on button B pressed||``: the same loop, but set pixel **9 − index** to **black**.

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

**Download**: **A** ignites, **B** retracts.
Two strips plugged together? Use **20** and **19** instead of **10** and **9**.

## Step 8: A flash when you swing

In ``||input:on shake||``: ``||neopixel:show color||`` **white**, pause **80** ms, show color **blue**.

```blocks
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
input.onGesture(Gesture.Shake, function () {
    strip.showColor(neopixel.colors(NeoPixelColors.White))
    basic.pause(80)
    strip.showColor(neopixel.colors(NeoPixelColors.Blue))
})
```

## Step 9: Is the blade on?

Make a variable **lit**. Set it to **true** at the end of button A, and to **false** at the start of button B.

```blocks
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
```

## Step 10: Tilt to change the colour

In ``||basic:forever||``, ``||logic:if||`` **lit**: set **hue** to ``||math:map||`` acceleration **x** from -1023 to 1023
to 0 to 359. Then ``||neopixel:show color||`` ``||neopixel:hue||`` **hue**.

```blocks
let hue = 0
let lit = false
let strip = neopixel.create(DigitalPin.P1, 10, NeoPixelMode.RGB)
strip.setBrightness(40)
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

## Step 11: Swing your lightsaber

**Download**. Press **A**, tilt left and right, and swing.
No cable? Plug in the battery pack after the download.

## Well done! @showdialog

Your lightsaber ignites, retracts, flashes and changes colour.
More challenges are on the [project page](https://www.startabit.com/projects/microbit-rainbow-lightsaber/#challenges).

```package
neopixel=github:microsoft/pxt-neopixel
```
