# EngNotebook3

## Table of Contents
* [TableofContents](#TableofContents)
* [CircuitPython](#CircuitPython)
* [LedGlow](#LedGlow)
* [Servo](#Servo)
* [DistanceSensor](#DistanceSensor)
* [MotorControl](#MotorControl)
* [AdvancedCAD](#AdvancedCAD)
* [Launcher](#Launcher)
* [SwingArm](#SwingArm)
* [Multi-PartStudio](#Multi-PartStudio)

# CircuitPython

## LedGlow

### Description

The goal of this assignment was to make the neopixel led on the Metro M4 to glow red (I chose purple). If you put the code in the regular way it glows blue so you have to download the neopixel file into your CircuitPython file to make it work.

### Code

```python
import board
import neopixel

dot = neopixel.NeoPixel(board.NEOPIXEL, 1)
dot.brightness = 0.1 

print("Tis Violet!")

while True:
    dot.fill((128, 0, 128))
    
```

### Image/Wiring

No wiring needed (led is already on the board)

### Reflection

use "import" to import stuff mainly neopixel to include the led on the board. M4 is a new board so there isn't a lot of code online with it. This makes it so you have to use different code. [I used the bundle for version 7](https://circuitpython.org/libraries)

## Servo

### Description

The goal of this assignment was to make a servo rotate between 0 and 180 degrees. The servo can be connected to any one of the D pins on the board.

### Code

```python
import time
import board
import pwmio
from adafruit_motor import servo

# create a PWMOut object on Pin D2.
pwm = pwmio.PWMOut(board.D2, duty_cycle=2 ** 15, frequency=50)

# Create a servo object, my_servo.
my_servo = servo.Servo(pwm)

while True:
    for angle in range(0, 180, 5):  # 0 - 180 degrees, 5 degrees at a time.
        my_servo.angle = angle
        time.sleep(0.05)
    for angle in range(180, 0, -5): # 180 - 0 degrees, 5 degrees at a time.
        my_servo.angle = angle
        time.sleep(0.05)

```

### Image/Wiring

![frank](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/servo-motor-with-arduino-uno-wiring-diagram-schematic-circuit-tutorial-featured-image.png?raw=true)
servo wiring

### Reflection

No onstacles to talk about here. You must import files you need in your code from the circuit python library.

## DistanceSensor

### Description

The goal of this assignment was to have the neopixel on the board fade between colors based on how far away it is from any given object (red for closest, green for farthest away).

### Code

```python
#credit grant for code

import adafruit_hcsr04
import neopixel
import time
import board
import simpleio #imports

sonar = adafruit_hcsr04.HCSR04(trigger_pin=board.D3, echo_pin=board.D2) #set pins for ultrasonic sensor
holylight = neopixel.NeoPixel(board.NEOPIXEL, 1) #set nexopixel to holylight
holylight.brightness = 0.1 #light brightness
red = 0
green = 0
blue = 0 #set variables to 0

while True:
    try:
        cm = sonar.distance
        print((sonar.distance)) #print the distance in cm
        time.sleep(0.01)
        if cm < 5:
            holylight.fill((255, 0, 0)) #red
        if cm > 5 and cm < 20:
            red = simpleio.map_range(cm,5,20,255,0)
            blue = simpleio.map_range(cm,5,20,0,255) #fade from red to blue
            holylight.fill((red, 0, blue))
        if cm > 20 and cm < 35:   
            blue = simpleio.map_range(cm,20,35,255,0)
            green = simpleio.map_range(cm,20,35,0,255) #fade from blue to green
            holylight.fill((0, green, blue))
        if cm > 35:
            holylight.fill((0, 255, 0)) #green
    except RuntimeError:
        print("Retrying!")
    time.sleep(0.1)

```

### Image/Wiring

![ultrasonicsensor](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/ultrasonicsensor.jpg?raw=true)
ultrasonic sensor wiring

### Reflection

It was hard to find code online with the M4 board. Ultrasonic sensor wire placements are different on the M4, you use pins like D2 and D3 as shown in the code. [Link to help with how to use simpleio](https://docs.circuitpython.org/projects/simpleio/en/latest/api.html)

## MotorControl

### Description

### Code

```python
# credit Kazuo Shinozaki

import board               #[lines 1-4] Importing neccesary libraries
import time
from analogio import AnalogOut, AnalogIn
import simpleio

motor = AnalogOut(board.A1) #[lines 5 & 6] Definining the motor and potentiometer
pot = AnalogIn(board.A0)

while True:
    print(simpleio.map_range(pot.value, 96, 65520, 0, 65535)) #Print mapped potentiometer value to motor inputs
    motor.value = int(simpleio.map_range(pot.value, 96, 65520, 0, 65535)) #Write the mapped value to motor
    time.sleep(.1)                                                      #So that the serial monitor works

```

### Image/Wiring

### Reflection

[helpful youtube video](https://www.youtube.com/watch?v=IJZ7SV_BkBs) and [Kazuo's Engineering Notebook](https://github.com/kshinoz98/CircuitPython.git), both of these are helpful for understanding motor control. Also got my code from his Engineering Notebook.

# AdvancedCAD

## Launcher

### Description

Make a launcher with a partner with each of you doing different parts and assembling together. Mates used to show how the key will affect the spinner.

### Image

![ring](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20092207.png?raw=true)
ring & spinner image
![key](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20092305.png?raw=true)
key & prop image

### [FinishedVideo](https://drive.google.com/file/d/1pwKUcreDCwL1iU4KaxW_-eUiIHNnaJTY/view?usp=sharing)

### [Evidence](https://cvilleschools.onshape.com/documents/43e557e4899c0d26b8889ae2/w/db816542b6a476e7ac5dfa93/e/76481082c5ed7d7019185ca2?renderMode=0&uiState=6356956988b2b44b5b584898)

### Reflection

Versions are the history of your document and branches are your personal sandbox to use as a testing ground without ruining the current build. No obstacles to be talked about. The key is too short as a useful part in real life so I recommend you double the length.

## SwingArm

### Description

Create a build based on an image and a few given dimensions. After the build is done you'll use the variables to change the size and shape of your build how the instructions tell you. Hopefully you dimensioned things right!

### Image

![P1](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20094136.png?raw=true)
step 1 swing arm
![P2](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20094058.png?raw=true)
step 2

### [Evidence](https://cvilleschools.onshape.com/documents/1bce9dcb2b34c53888d76b6d/w/575f824c6e28c5a80919e26c/e/dc45af6a9212fe7a46e9f887?renderMode=0&uiState=635696af2b6ade1f6e631b76)

### Reflection

I missed creating one of the extrudes in the build. Pay full attention to the full build picture so you dont miss any parts of the build. Keep in mind you have to change values so you want that to affect the build the intended way (such as distancing a circle 5mm from another circle instead of just putting the total value.)

## Multi-PartStudio

### Description

The swing arm but on steroids, it doesn't give you a part to create first so it's your job to figure out the best one. You make all the parts in the same part studio which is what differs from the swing arm since that didn't really have separate parts.

### Image

![frank](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20095002.png?raw=true)
step 1 multi-part studio
![frank](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20095034_Q2.png?raw=true)
step 2
![frank](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20095242_Q3.png?raw=true)
step 3
![frank](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202022-10-24%20095312_Q4.png?raw=true)
step 4

### [Evidence](https://cvilleschools.onshape.com/documents/25363d97db20e1fd1fba4177/w/56566671744e1cf16f5923a2/e/5efb0aabc648c8b244fa1e18?renderMode=0&uiState=6357e099285ef11c0bbf4cbe)

### Reflection

Start with the bottom cap because it's basically the base and the top cap is a more complicated copy of it. If doing this again, make sure to find the easiest starting part and pay attention to how you dimension things off of each other with the changes in mind. shift+s to create a sketch aved a lot of time, so did linear patterns on the holes.
