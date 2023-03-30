---
permalink: /system-design/
title: "System Design"
excerpt: ""
last_modified_at: 
toc: true
---

## System Architecture

### Diagram

![System Architecture Diagram]({{ site.url }}{{ site.baseurl }}/assets/images/system-arch-diagram.png)

This diagram visualises how each component of our project works together, as well as groups them into the following sections: User, Front-End, Back-End, Data Store.

### Config Wizard MFC App

This is the client-facing front-end application, that the user will actually interact with and use the UI to customise their settings for MotionInput, eg. mode, mouse sensitivity, button actions. The settings are then saved in the JSON config files in the data store. The user can then launch the MotionInput executable from the app.

### MotionInput

This is the backend and the heart of the project. It is the software that contains all of the libraries for pose detection, speech and gestures, and is the layer that then communicates with the OS to perform actions such as mouse movement/clicks/presses, keypresses. The actual gesture calculations are done here (eg. Forcefields), which then trigger certain events (eg. Mouse Click).

### MediaPipe Hand Landmark Detection

This is where all of the hand tracking is done from the webcam feed using MediaPipe, which returns 21 landmarks of different points on the hand. We can then use these landmarks to calculate gestures using the distance between them, depth values, etc.

### Data Store

This is where all of the configuration data is stored for MotionInput and the MFC, in the form of JSON files. We store general settings like camera number, as well as data for events and gestures, such as sensitivity, forcefields max/min depth, which actions to perform on level changes.

### Unity Game

This is on the application layer which is separate to the others and can be played independently. It only receives mouse actions and key press information, which is handled entirely by MotionInput.

## Packages and APIs

- [Google MediaPipe](https://google.github.io/mediapipe/)
- [OpenCV](https://opencv.org/)