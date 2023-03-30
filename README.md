# EngNotebook3

## Table of Contents
* [TableofContents](#TableofContents)
* [CircuitPython](#CircuitPython)
* [Led Glow](#LedGlow)
* [Servo](#Servo)
* [Distance Sensor](#DistanceSensor)
* [Motor Control](#MotorControl)
* [Temperature Sensor](#TemperatureSensor)
* [Rotary Encoder](#RotaryEncoder)
* [Photointerrupter](#Photointerrupter)
* [CAD](#AdvancedCAD)
* [Launcher](#Launcher)
* [Swing Arm](#SwingArm)
* [Multi-Part Studio](#Multi-PartStudio)

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

The goal of this assignment was to use a potentiometer to control motor speed. Set the motor as the output and the potentiometer as the input. Make sure to use a AA battery pack with 6V total (4 batteries) so you don't fry your board and see the magic smoke.

### Code

```python
# credit Kazuo Shinozaki

import board               #[lines 1-4] Importing necessary libraries
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

![dc motor](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%20(16).png?raw=true)
Motor Control Wiring Credit: [Grant](https://github.com/ggastin30/CPython.git)

### [Video Evidence](https://drive.google.com/file/d/1tXgbfAnnkSYnp6516ewDwkUUgyvBpg1y/view?usp=sharing)

### Reflection

[helpful youtube video](https://www.youtube.com/watch?v=IJZ7SV_BkBs) and [Kazuo's Engineering Notebook](https://github.com/kshinoz98/CircuitPython.git), both of these are helpful for understanding motor control. Also got my code from his Engineering Notebook.

## TemperatureSensor

### Description

Use a TMP36 Temperature Sensor and an i2c LCD Screen to print a statement based on the temperature on the lcd screen and print the temperature values in the serial monitor 

    - If the temperature is within a desired range, print a message "It feels great in here"
    
    - If it is too cold, print "brrr Too Cold!"
    
    - If it is too hot, print "Too Hot!"

### Code

```python

import board
import analogio
import time
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface


TMP36_PIN = board.A0  # Analog input connected to TMP36 output.

i2c = board.I2C()
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16) #0x27 or 0x3f depending on which one works

# Function to simplify the math of reading the temperature.
def tmp36_temperature_C(analogin):
    millivolts = analogin.value * (analogin.reference_voltage * 1000 / 65535)
    return (millivolts - 500) / 10

# Create TMP36 analog input.
tmp36 = analogio.AnalogIn(TMP36_PIN)
desired_temp_min = 70
desired_temp_max = 78

while True:
    # Read the temperature in Celsius.
    temp_C = tmp36_temperature_C(tmp36)
    # Convert to Fahrenheit.
    temp_F = (temp_C * 9/5) + 32
    # Print out the value and delay a  few seconds before looping again.
    print("Temperature: {}C {}F".format(temp_C, temp_F))
    time.sleep(3.0)
    if temp_F >= desired_temp_min and temp_F <= desired_temp_max:
        lcd.clear()
        lcd.print("It feels great  in here")
    if temp_F < desired_temp_min:
        lcd.clear()
        lcd.print("brrr too cold")
    if temp_F > desired_temp_max:
        lcd.clear()
        lcd.print("Too hot")
    time.sleep(1.0)

```

### Image/Wiring

![tempwire](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202023-03-14%20100722.png?raw=true)

### [Video](https://drive.google.com/file/d/1uhP3nvMrXnHAE0zIdmMa3KRYp5l72kAJ/view?usp=sharing)

### Reflection

Get the code for the tmp36 sensor down before including the lcd. tmp36 doesn't have its own library file use analogio like for a potentiometer.
remember to switch the lcd address if it doesnt work and unplug the lcd from the 5V if the metro m4 wont show up in file explorer. [very helpful page](https://learn.adafruit.com/tmp36-temperature-sensor/tmp36-with-circuitpython)

## RotaryEncoder

### Description

The assignment is to use an encoder, an LCD display, and three LEDs (red, yellow and green) to create a menu based traffic light control. Turning the encoder will change the LCD display to either caution, go, or stop, but the LED will only line up with the corresponding color if you press down the encoder.

### Code

``` python

import rotaryio
import board
import digitalio
import neopixel
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface #imports

i2c = board.I2C()

# some LCDs are 0x3f... some are 0x27.
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)

led: neopixel.Neopixel = neopixel.NeoPixel(board.NEOPIXEL, 1) # initialization of hardware
print("neopixel")

led.brightness = 0.1

button = digitalio.DigitalInOut(board.D12)
button.direction = digitalio.Direction.INPUT
button.pull = digitalio.Pull.UP

colors = [("stop", (255, 0, 0)), ("caution", (128, 128, 0)), ("go", (0, 255, 0))]   #three different vars for colors

encoder = rotaryio.IncrementalEncoder(board.D10, board.D9, 2)   # pin 10, 9, and 12
last_position = None
while True:
    position = encoder.position #new var
    if last_position is None or position != last_position:
        lcd.clear()
        lcd.print(colors[position % len(colors)][0])
    if(not button.value):
        led[0] = colors[position % len(colors)][1]
    last_position = position

```

Credit to [River L](https://github.com/rivques?tab=repositories) for code

### Image/Wiring

![rotarywiring](https://github.com/Cooper-Moreland/EngNotebook3/blob/main/Screenshot%202023-03-24%20093258.png?raw=true)

### [Video](https://drive.google.com/file/d/1psrgAxanX-VfuovkOC3A1tHfGWCb9t6h/view?usp=sharing)

### Reflection

Wiring was easy for this assignment, but the code was the tought part. River Lewis heped me understand it with his commented code, his engineering notebook is linked above.

## Photointerrupter

### Description

### Code

``` python

import time
import digitalio
import board

photoI = digitalio.DigitalInOut(board.D7)
photoI.direction = digitalio.Direction.INPUT
photoI.pull = digitalio.Pull.UP

last_photoI = True
last_update = -4

photoICrosses = 0

while True:
    if time.monotonic()-last_update > 4:
        print(f"The number of crosses is {photoICrosses}")
        last_update = time.monotonic()
    
    if last_photoI != photoI.value and not photoI.value:
        photoICrosses += 1
    last_photoI = photoI.value
    
```

### Image/Wiring

### [Video]

### Reflection

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
