# Rainbow lightsaber

### @explicitHints false

## Introduction @showdialog

Your LED strip has **addressable LEDs**: every LED has a number, its address, and a tiny chip.
The Micro:bit sends the colours on one data wire, and each LED takes its own.

One strip has **10 LEDs**, numbered **0 to 9**. LED 0 is next to the wires.

![Wire it: red to 3V, yellow to pin 1, black to GND](https://startabit.startabit.workers.dev/projects/microbit-rainbow-lightsaber/images/2-wire-it.png)

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

## Next: swing it @showdialog

Well done! Level 4 is on the [project page](https://startabit.startabit.workers.dev/projects/microbit-rainbow-lightsaber/#program):
the blade flashes when you shake the Micro:bit, and changes colour when you tilt it.

```package
neopixel=github:microsoft/pxt-neopixel
```
