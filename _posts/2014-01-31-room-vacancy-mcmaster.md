---
layout: post
title:  "Find empty rooms"
date:   2014-01-29
excerpt: "Find empty rooms on campus"
project: true
tag:
- project
---

## Check room vacancy at McMaster
A problem I cam across at McMaster is the lack of information about when a class takes place in a certain room. Group study rooms are limited so a lot of people retreat to studying in classrooms when a lesson is not taking place. Classrooms are great for studying with a group of friends but there is nothing worst than a professor kicking you out because there is a class coming up.
I have had this problem often so I decided to do something about it. I have created a program that is able to tell you if a lesson is taking place, and a list with when the next lesson will start. This tool enables students to plan their use of the room accordingly. 

The software is programmed in Java, I have used libraries such as jSoup and Joda time to parse and handle the publicly available data. The program parses thought the master timetable from McMaster ( http://registrar.mcmaster.ca/scheduling/coursett.html) and it looks at all the times when no lesson is taking place. 

The program follows the MVC model, and it takes advantage of the OOP functionality of Java, keeping the data in a more organized and efficient to work with way. 

The program is meant to have as simple as possible of a GUI. The only values the user needs to pick is the building and the room, the rest is taking care of. The GUI is programmed in jSwing , the element code is manually entered and not graphically. To make any changes to the GUI take a look at CalendarView.java.


The next step in the lifetime of the product is to bring it to a greater population by not forcing the user to download the compiled java file but instead create a web based input and let the program act as a backend running on a server.

![GUI](https://4.bp.blogspot.com/-7UFw0d_sJ0s/UuxZSCHNoQI/AAAAAAAAAHo/fCNUqxaQqX8/s1600/Screenshot+2014-01-31+21.15.39.png)


Feel free to download and use the program and code as desired. Setup instructions are in comments. A NetBeans project is posted bellow.
[Unzip and import](https://dl.dropboxusercontent.com/u/2470102/Calendar.zip)
[Or here is the compiled version](https://dl.dropboxusercontent.com/u/2470102/Is%20there%20a%20room%20in%20here%3F.zip)
