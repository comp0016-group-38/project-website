---
permalink: /implementation/
title: "Implementation"
excerpt: ""
last_modified_at:
toc: true
---

## MotionInput

### Wrist-Tracking

As we only want to activate wrist-tracking when the wrist is visible in the camera feed, we had to add a new primitive to do this in the `hand` module.

```python
def _check_wrist_visible(self):
    self._primitives["wrist_visible"] = ("wrist" in self._landmarks)
```

With this added, we could then add the wrist tracking event.

```python
class WristTrackingEvent(SimpleGestureEvent):
    """Each frame the wrist gesture is active, the 'move' trigger is called with the coordinates of the wrist
    
    [trigger types]:
        "move": called every frame when the event is active (so when the hand is facing the camera) with input of (palm_center_x_coordinate, palm_center_y_coordinate). coordinates in percentage of the camera view relative to the left top corner
    [bodypart types]:
        "hand": which hands movement is tracked
    [gestures types]: 
        "wrist_visible": 
    """
    _event_trigger_types = {"move"}
    _gesture_types = {"wrist_visible"}
    _bodypart_types = {"hand"}

    def __init__(self):
        self._gestures = {"hand": {"wrist_visible": None}}
        super().__init__(WristTrackingEvent._gesture_types, WristTrackingEvent._event_trigger_types,
                         WristTrackingEvent._bodypart_types)

    def update(self):
        hand_position = self._gestures["hand"]["wrist_visible"].get_last_position()
        wrist_point = hand_position.get_landmark("wrist")
        self._event_triggers["move"](wrist_point[0], wrist_point[1])

    def _check_state(self) -> None:
        self._state = self._gestures["hand"]["wrist_visible"] is not None
```

In the `desktop_mouse.py` gesture handler file, we had to modify the `move_cursor` function in the `AOIMouse` class, to add the spasm reduction mouse smoothing functionality.

```python
def move_cursor(self, cam_x: float, cam_y: float):
    """Translate the camera coordinates into the AOI coordinates. Smooth the movement of the mouse by averaging out the coordinates of the last few frames (as configured). Move the mouse accordingly."""
    old_position = self._cursor_xy

    adjusted_sensitivity = 0.7 + (self._sensitivity / 10)  # sensitivity 3 is 1.0, sensitivity 10 is 1.7
    cam_x = (cam_x - 0.5) * adjusted_sensitivity + 0.5
    cam_y = (cam_y - 0.5) * adjusted_sensitivity + 0.5

    cursor_stabiliser_factor = 0.9
    smoothed_position_x = cursor_stabiliser_factor * old_position[0] + (1 - cursor_stabiliser_factor) * cam_x
    smoothed_position_y = cursor_stabiliser_factor * old_position[1] + (1 - cursor_stabiliser_factor) * cam_y

    self._cursor_xy = [smoothed_position_x, smoothed_position_y]

    x, y = self._aoi.convert_xy(smoothed_position_x, smoothed_position_y)
    print(x, y)
    self._mouse.position = self._smooth_mouse(x, y)
```

### Forcefields

First, we had to create a new type of event which we call `ForcefieldEvent`. In here we do the calculations for the forcefield interaction, and invoke the button actions.

We retrieve the values that we need from the config file in the `init` function.

```python
buttons = config.get_data(f"events/contractures/buttons_{hand}")
self._buttons = [[value, False] for value in buttons]
self._num_buttons = len(buttons)

# read calibrated depth values from config
depth_start = config.get_data("events/contractures/forcefields_start")
depth_end = config.get_data("events/contractures/forcefields_end")
# depth_range = config.get_data("events/contractures/forcefields_depth_range")

self._depth_boundaries = self._calc_depth_boundaries(self._num_buttons, depth_start, depth_end)

self._current_level = 0
self._triggered_level = 0

# store buffer of depth values at each frame
self._depth_buffer_size = config.get_data("events/contractures/buttons_activation_time")
self._depth_buffer = deque(maxlen=self._depth_buffer_size)
```

To calculate the level depth boundaries, we take even intervals between the min/max values.

```python
def _calc_depth_boundaries(self, num_buttons, depth_start, depth_end):
    boundaries = []
    depth_range = depth_end - depth_start
    depth_level_interval = depth_range / (num_buttons + 1)
    for i in range(1, num_buttons + 1):
        level_boundary = depth_start + i * depth_level_interval
        boundaries.append(level_boundary)
    print(boundaries)
    return boundaries
```

In the `update` function, this is where we call the button actions. We keep track of which buttons are being pressed in a 2D array in this format `[[key, status]]`. Additionally, we calculate the depth of the hand (explained more in Algorithms) to determine the current level based on the calculated thresholds. Whenever a new level is reached, the corresponding button action is started, and its status is saved as `pressed=True.`

```python
def update(self):
    if self._state:
        hand = self._gestures["hand"]["wrist_visible"].get_last_position()
        self._get_hand_depth_level(hand)

        # add/remove from buffer
        self._depth_buffer.append(self._current_level)
        if self._depth_buffer.count(self._current_level) < self._depth_buffer_size:
            # not all items in buffer are same as current level
            return

        if self._current_level > 0:
            # press key for this level boundary
            for index, button in enumerate(self._buttons):
                level = index + 1
                if level == self._current_level:
                    # must be going forwards
                    if button[1] is False and self._current_level > self._triggered_level: 
                        self._trigger_button(button)
                        self._triggered_level = level
                elif button[1] is True:
                    self._release_button(button)
        elif self._current_level == 0:
            # release all keys
            for button in self._buttons:
                if self._triggered_level > 0:
                    self._triggered_level = 0
                if button[1] is True:
                    self._release_button(button)
```

Event which defines the hand and triggers in `events.json`:

```json
"hand_forcefield_left_hand": {
    "type": "ForcefieldEvent",
    "args": {
        "hand": "left"
    },
    "bodypart_names_to_type": { "Left": "hand" },
    "triggers": {
        "key_press": [ "Keyboard", "key_down" ],
        "key_release": [ "Keyboard", "key_up" ],
        "mouse_left_click": [ "DesktopMouse", "left_click" ],
        "mouse_left_press": [ "DesktopMouse", "left_press" ],
        "mouse_left_release": [ "DesktopMouse", "left_release" ],
        "mouse_right_click": [ "DesktopMouse", "right_click" ],
        "mouse_right_press": [ "DesktopMouse", "right_press" ],
        "mouse_right_release": [ "DesktopMouse", "right_release" ],
        "mouse_double_click": [ "DesktopMouse", "double_click" ]
    }
},
```

### Punching

This interaction is very similar to forcefields, however the gestures are slightly different. Here we need to make sure that the punching action is correct, with a clenched fist, so we use the `fist` gesture.

```json
"fist": {
    "index_folded": true,
    "middle_folded": true,
    "ring_folded": true,
    "pinky_folded": true
},
```

Instead of using the side height of the hand, now we are using the knuckle height of the fist. So we had to add the `knuckle_height` primitive in the `hand` module.

```python
def get_knuckle_height(self) -> Optional[float]:
    """Returns the distance between the knuckles"""
    if "pinky_base" not in self._landmarks or "index_upperj" not in self._landmarks:
        return None
    return self.get_landmarks_distance("pinky_base", "index_upperj")
```

### Modes

We added the following modes to be used in MotionInput:

- `contractures_buttons_nav_left`
- `contractures_buttons_nav_right`
- `contractures_buttons_nav_left_speech`
- `contractures_buttons_nav_right_speech`
- `contractures_buttons_forcefields`
- `contractures_buttons_punching`

#### Buttons

This mode type allows for either forcefields/punching gestures. For each of these gestures, there is the ability to add up to 2 button actions per hand. This includes both keypress/mouse actions.

You can configure the buttons settings in the `config.json` file, in `events/contractures`:

```json
"buttons_activation_time": 5,
"buttons_left": [
    "mouse_left_click",
    "mouse_left_press"
],
"buttons_right": [
    "w",
    "q"
],
"forcefields_end": 0.16714291310390497,
"forcefields_start": 0.1254580090319135,
"punching_end": 0.27385270935686273,
"punching_start": 0.06424507998477785,
```

- `buttons_activation_time`: number of frames that you must hold the gesture position for the button to trigger, any positive integer - 0 is instant.
- `buttons_left`: array containing button actions for left hand, `[level 1 action, level 2 action]`.
- `buttons_right`: array containing button actions for right hand, `[level 1 action, level 2 action]`.
- `forcefields_start`: min depth value for forcefields (from calibration)
- `forcefields_end`: max depth value for forcefields (from calibration)
- `punching_start`: min depth value for punching (from calibration)
- `punching_end`: max depth value for punching (from calibration)

#### Navigation

This mode allows the user to have 1 hand for controlling the mouse movement, and 1 hand for doing button interactions (using Forcefields). The main hand is the one that controls the mouse and is determined by the name of the mode in `mode_controller.json` - if it contains `_right`, the main hand is the right hand, and the same goes for the left hand.  The mouse movement is set to track the wrist movement, and has built in noise reduction algorithms to smooth any spasms/hand jitter.

The other hand is then set to use forcefields and can have up to 2 button actions (as shown in the previous section). Set these actions in the corresponding field, eg. right hand - `buttons_right`.

You can control the mouse sensitivity and spasm reduction in the `config.json` file, in `events/contractures`:

```json
"wrist_mouse_sensitivity": 3,
"wrist_spasm_reduction": 10
```

- `wrist_mouse_sensitivity`: the sensitivity of the mouse, in the range [0, 10], default is 3.
- `wrist_spasm_reduction`: the amount of spasm reduction to apply to the mouse movement, in the range [0, 10].

You can also enable some speech commands to make navigation easier, by adding `_speech` at the end of the mode. Then, in conjunction with the forcefields actions, you can use the following speech commands:

| Phrase         | Action                                 |
|----------------|----------------------------------------|
| "click"        | Mouse Left Click                       |
| "right click"  | Mouse Right Click                      |
| "double click" | Mouse Double Click (Left Mouse Button) |

### Calibration

![Calibration]({{ site.url }}{{ site.baseurl }}/assets/images/calibration-stages.png)

To make sure that the gestures work correctly, the user must run calibration when using any of the modes. 

Will walk the user through a simple step-by-step process where they need to hold a certain position for a few seconds (eg. for forcefields → hold hand close to you, hold hand far away from you). There is a GUI with easy to understand text prompts. The min/max depth values will be saved, so that the depth thresholds for each level can be dynamically calculated.

To run calibration, the following values are set in the`config.json` file, in `modules/hand`.

```json
"run_forcefields_calibration": false,
"run_punching_calibration": false
```

Only one of these is needed to be calibrated at a time, either forcefields or punching. To run calibration on launch, all you need to do is set the field to `true`.

### Example Config

```json
"contractures": {
    "buttons_activation_time": 5,
    "buttons_left": [
        "i",
        "k"
    ],
    "buttons_right": [
        "o",
        "l"
    ],
    "forcefields_end": 0.16714291310390497,
    "forcefields_start": 0.1254580090319135,
    "punching_end": 0.27385270935686273,
    "punching_start": 0.06424507998477785,
    "wrist_mouse_sensitivity": 3,
    "wrist_spasm_reduction": 10
},
```
## MFC

![MFC]({{ site.url }}{{ site.baseurl }}/assets/images/mfc.png)

### Set mode

Based on which buttons are checked, we set the current mode in the `config.json` file.

```cpp
if (m_modeNavigation.GetCheck()) {
	if (m_mainHandLeft.GetCheck() && globalSpeech) globalCurrentMode = MODE_NAV_LEFT_SPEECH;
	else if (m_mainHandRight.GetCheck() && globalSpeech) globalCurrentMode = MODE_NAV_RIGHT_SPEECH;
	else if (m_mainHandLeft.GetCheck()) globalCurrentMode = MODE_NAV_LEFT;
	else if (m_mainHandRight.GetCheck()) globalCurrentMode = MODE_NAV_RIGHT;
}
else if (m_modeButtons.GetCheck()) {
	if (m_gestureForcefields.GetCheck()) globalCurrentMode = MODE_BUTTONS_FORCEFIELDS;
	else if (m_gesturePunching.GetCheck()) globalCurrentMode = MODE_BUTTONS_PUNCHING;
}
```

### Set parameters

There are buttons to enable/disable fields, as well as sliders for ranges, and drop-down selects for button actions.

## Unity Game

### Assets

The 3 Unity asset packs we used were “**[Snow Forest Pack Cute Series](https://assetstore.unity.com/packages/3d/environments/snow-forest-pack-cute-series-197174)”**, **“[Z ! Low Poly Nature Pack](https://assetstore.unity.com/packages/3d/environments/fantasy/z-low-poly-nature-pack-144332)”** and **“[Low Poly Animated Animals](https://assetstore.unity.com/packages/3d/characters/animals/low-poly-animated-animals-93089)”**.

![Assets]({{ site.url }}{{ site.baseurl }}/assets/images/unity-assets.png)

## Generating Map

The snowy mountain scene was imported from **“Snow Forest Pack Cute Series”**. The other 3 scenes were made with the **“Z ! Low Poly Nature Pack”** asset pack. All the animals used were prefabs from **“Low Poly Animated Animals”**.

![Assets]({{ site.url }}{{ site.baseurl }}/assets/images/unity-maps.png)

## Movement

In each scene, the user can pan around using mouse. This function takes in the input of the mouse positions both in the vertical and horizontal direction. Here the sensitivity can be adjusted and is a scalar for the x and y input.

```csharp
turn.x += Input.GetAxis("Mouse X") * sensitivity;
turn.y += Input.GetAxis("Mouse Y") * sensitivity;
transform.localRotation = Quaternion.Euler(-turn.y, turn.x, 0);
```

Also, in each scene the cursor disappears when users are playing and appears otherwise to click buttons. This is done by the following:

```csharp
 Cursor.lockState = CursorLockMode.Locked; // cursor disappear
 Cursor.lockState = CursorLockMode.None; // cursor appear
```

## Animations

To change animal animation we change its state. The states are represented by bools and are easy to manipulate. The following code is an example of setting the state of an animal to running.

```csharp
animator.SetBool("isRunning", true);
```

There is a function that is called if the animal is stuck in the obstacle/terrain. This function randomly selects a point on the Navmesh at a certain radius from the animal which is then used to set a new destination.

```csharp
private Vector3 RandomNavmeshLocation(float radius) {
    Vector3 randomDirection = Random.insideUnitSphere * radius;
    randomDirection += transform.position;
    UnityEngine.AI.NavMeshHit hit;
    Vector3 finalPosition = Vector3.zero;
    if (UnityEngine.AI.NavMesh.SamplePosition(randomDirection, out hit, radius, 1)) {
        finalPosition = hit.position;            
    }
    return finalPosition;
}
```

## Interactions

For each scene in the game, there are two interactions with the animals. Each interaction is triggered by a key press; the key presses in this game are “j” and “k”. If the animal not yet collide with the user, pressing “j” makes the animals only perform “run” or “swim” state and run or swim towards the user/camera.

```csharp
if (Input.GetKey("j") && !hasCollided){
	// ensure the animals are facing and moving towards correct position
  agent.transform.LookAt(cam.transform); 
  agent.destination = cam.transform.position;

	// change state
  animator.SetBool("isRunning", true);
  foreach (string state in states){
    if (state != "isRunning"){
      animator.SetBool(state, false);
    }
  }
}
```

When “j” key triggers animals run towards camera and has a distance smaller than 5, it stops the animal from running closer by setting the variable hasCollided to true. And set the animals to other state. 

```csharp
if (dist < 5){
  hasCollided = true;
}
else{
  hasCollided = false;
}
```

Pressing “k” stops their movement and some animals who has attacking state will perform an attacking animation. 

```csharp
else if (Input.GetKey("k")){
  animator.SetBool("isAttacking", true);
  foreach (string state in states){
    if (state != "isAttacking"){
      animator.SetBool(state, false);
    }
  }
  agent.speed = 0;
}
```
