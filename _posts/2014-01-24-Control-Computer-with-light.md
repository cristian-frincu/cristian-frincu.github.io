---
layout: post
title:  "Use light as a mouse"
date:   2014-01-24
excerpt: "Using two light sensors as a mouse"
project: true
tag:
- project
---

## Controlling the computer using two passive light sensors and an Arduino
I have recently been working on a method of controlling the input to a computer using two light sensors. The sensors are connected to an Arduino Nano. The input from the two sensors is processed with a Python script.

By using two sensors located at 90 deg. to each other I was able to get the x and y coordinate from each one in turn allowing the user to essentially draw by just moving their hands around.

The biggest challenge I had with the design was designing a way to auto-calibrate the origin on the screen.
Since I was just measuring the voltages from the sensors, depending on the light intensity the origin had to be readjusted. So depending in what orientation in regards to the main source of light the sensors were facing, the read values were different.
To overcome this issue I decided to take a few sample points at the beginning of the reading which served as an average that indicated the starting light intensity and all other readings have been done in respect to those values.

Due to the very small variations in light intensity, I decided the values need to be increased and I decided to increase them exponentially that way the hand of the user does not need to move so much but it could still reach the corners of the screen when controlling the mouse.


This is a video which demonstrates the product still in its early stages.

i<iframe width="420" height="315" src="https://youtu.be/2M5YJC7XHHM" frameborder="0" allowfullscreen></iframe>

The python code is bellow, to make it work with the Arduino it needs to send the signal from the two sensors as one charString values separated by the |. The rest is done by the Python script


```python
import serial
from graphics import *
win = GraphWin("Graph",1100,750)

ser = serial.Serial('/dev/cu.usbserial-A6022ZCK',9600,bytesize=8, parity='N', stopbits=1, timeout=1) 
ser.close()
ser.open()
x=20
count = 0
pastPoint = Point(0,0)
resets=0
inputVert=0
inputHor=0



def calibrateCoordinates():
	averageX=0
	averageY=0
	
	for i in range(60):

		try:
			input= ser.read(9)
			parsed = input.split("|")
			print int(parsed[0])
			print int(parsed[1])
			if (i >=10) : 
				averageX += int(parsed[0])
				averageY += int(parsed[1])
			
		except ValueError:
			print "Error at error parsing"
			continue
	print averageX
	print averageY
	average=[averageX/50,averageY/50]
	print "---------------------------"
	return average
		



calibrationCoordinates = calibrateCoordinates()
print calibrationCoordinates[0]
print calibrationCoordinates[1]


while True:
	try:
		input= ser.read(9)
		parsed = input.split("|")
		inputVert = int(parsed[0])
		inputHor = int(parsed[1])
		
	except ValueError:
		print "error: "
		print inputVert
		continue
		
	alteredX = int(500+calibrationCoordinates[0]-inputHor**1.13)
	alteredY = int(200-calibrationCoordinates[1]+(inputVert**1.13))
	print "x: "
	print (alteredX)
	print "y:"
	print (alteredY)
	print "\n"

	aColor = color_rgb(count,150,255)
	cleanColor = color_rgb(255,255,255)
	
	pt = Point (alteredX,alteredY) #multiply by a constant to amplify the input
	#pastPoint = Point (50,700)
	segment = Line(pastPoint,pt)
	segment.setFill(aColor)
	segment.draw(win)
	x+=1
	
	pastPoint = pt
	
	count +=1
	
	if (resets >=4):
		break
	
	
	if (count>= 255):
		count=0
		shape = Rectangle(Point(0,0), Point(1100,750))
		shape.setFill(cleanColor)
		shape.draw(win)
		resets+=1
ser.close()

def calibrateCoordinates():
	for i in range(5):
		try:
			input= ser.read(9)
			
		except ValueError:
			print "error at error parsing. This is good and what it should happen "
			continue
	averageX=0
	averageY=0
	
	for i in range(50):
		try:
			input= ser.read(9)
			parsed = input.split("|")
			averageX += int(parsed[0])
			averageY += int(parsed[1])
			
		except ValueError:
			print "error at error parsing. This is good and what it should happen "
			continue
		averageX=averageX/50
		averageY=averageY/50
```
