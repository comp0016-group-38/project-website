---
permalink: /implementation/
title: "Implementation"
excerpt: "How we created the project."
last_modified_at: 2022-05-27T11:59:26-04:00
toc: true
---

## MotionInput

[pic]

### Gestures

Hand pose detection using MediaPipe.

Wrist tracking → trigger every frame wrist is visible

Forcefields → wrist visible

Punching → fist

### Events

Gets customised values for gestures from config.json

depth start, end, button presses 

### Modes

- 2 types of modes, navigation and buttons
- Navigation is one hand wrist tracking one hand Forcefields/speech
- Buttons is both hands Forcefields/punching with 2 level triggers

contractures_nav_left

contractures_nav_right

- Use forcefield to do actions, click, keypress

contractures_nav_left_speech

contractures_nav_right_speech

- Added speech modes, simple for navigation mode, use wrist tracking with main hand and say “click”, “right click”, “double click” to do the action.

contractures_buttons_forcefields

contractures_buttons_punching

These are the 

```json
"contractures": {
  "buttons_activation_time": 1,
  "buttons_left": [
    "w",
    "a"
  ],
  "buttons_right": [
    "s",
    "d"
  ],
  "forcefields_end": 0.4207999565793383,
  "forcefields_start": 0.18948290718106434,
  "punching_end": 0.5,
  "punching_start": 0.1,
  "wrist_mouse_sensitivity": 10,
  "wrist_spasm_reduction": 5
},
```

### Button Triggers

Retrieve button trigger actions from config

Keeping track of which buttons are currently pressed in a 2d array [[key, status]]

Allow for mouse actions as well

Calculate hand depth/knuckle height and get current level based on thresholds

[code thresholds]

When it enters a new level, trigger button action and save status as pressed/True

### Calibration

[pics calibration window]

Everyone has different range of motion, different distance from camera, need to calibrate

Calibrate forcefields, punching

GUI display, intuitive and simple

Saves values to config.json

## MFC

![mfc.png]({{ site.url }}{{ site.baseurl }}/assets/images//mfc.png)

### Reading/writing to config.json

Nlohmann Jason library c++

### Set mode

Based on which buttons are checked, set the current mode

[code check buttons set modes]

### Set parameters

Sliders

Drop-down keys for buttons

### Launch with calibration

Set the parameters for calibration keys to true, read in MotionInput and run the correct calibration setup 

## Unity Game

### Assets

The 3 Unity asset packs we used were “**Snow Forest Pack Cute Series”**, **“Z ! Low Poly Nature Pack”** and **“Low Poly Animated Animals”**.

### Generating Map

The snowy mountain scene was imported from one of the asset packs. The other 3 scenes were made with the **“Z ! Low Poly Nature Pack”** asset ****pack. All the animals used were prefabs from **“Low Poly Animated Animals”**.

### Movement

In each scene, the user can pan around. This function takes in the input of the mouse both in the vertical and horizontal direction. Here the sensitivity can be adjusted and is a scalar for the x and y input.

```csharp
turn.x += Input.GetAxis("Mouse X") * sensitivity;
turn.y += Input.GetAxis("Mouse Y") * sensitivity;
transform.localRotation = Quaternion.Euler(-turn.y, turn.x, 0);
```

Also, in each scene the cursor disappears and the user can pan around using the mouse. This is done by locking the cursor.

```csharp
 Cursor.lockState = CursorLockMode.Locked;
```

### Animations

To change animal animation we change its state. The states are represented by bools and are easy to manipulate. The following code is an example of setting the state of an animal to running.

```csharp
animator.SetBool("isRunning", true);
```

### Interactions

For each scene in the game, there are two interactions with the animals. Each interaction is triggered by a key press; the key presses in this game are “j” and “k”. Pressing “j” makes the animals run/swim towards the user/camera if it is at least at a distance of 5 from the user/camera.

```csharp
if (Input.GetKey("j") && !hasCollided)
{
  agent.transform.LookAt(cam.transform); 
  agent.destination = cam.transform.position;

  animator.SetBool("isRunning", true);
  foreach (string state in states)
  {
    if (state != "isRunning")
    {
      animator.SetBool(state, false);
    }
  }
}
```

When the animal gets close to the camera when “j” is pressed, it stops the animal from getting closer by setting the variable hasCollided to true which is checked when “j” is pressed.

```csharp
if (dist < 5)
{
  hasCollided = true;
}
else
{
  hasCollided = false;
}
```

Pressing “k” stops the animals movement and some animals perform an attacking animation.

```csharp
else if (Input.GetKey("k"))
{
  animator.SetBool("isAttacking", true);
  foreach (string state in states)
  {
    if (state != "isAttacking")
    {
      animator.SetBool(state, false);
    }
  }
  agent.speed = 0;
}
```