---
permalink: /algorithms/
title: "Algorithms"
excerpt: ""
last_modified_at: 
toc: true
---

## MotionInput

### Wrist Tracking

Tracking the wrist point. Noise reduction of wrist point to reduce spasms

Laplacian smoothing

![applsci-09-05437-g001.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c48d62d-848c-405d-a44a-c892404ef1ef/applsci-09-05437-g001.png)

Gaussian filter on wrist points

![1200px-Gaussian_Filter.svg.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/233eb7dc-cefb-4b45-ade5-8de27e961b4b/1200px-Gaussian_Filter.svg.png)

Damping

### Triggers

### Forcefields

![2de77c27402c41cb8b39bb279d5bf86b.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6cbb53b6-ed55-4609-bc46-93e1e6f55083/2de77c27402c41cb8b39bb279d5bf86b.jpg)

Define the gesture to trigger on → when the wrist is visible

```python
# scripts/hand_module/hand_position.py
class HandPosition(Position):
		... 
		def _check_wrist_visible(self):
				self._primitives["wrist_visible"] = ("wrist" in self._landmarks)
```

```python
# scripts/gesture_events/forcefield_event.py
class ForcefieldEvent(SimpleGestureEvent):
    _gesture_types = {"wrist_visible"}
		...
```

Get the most recent position of the hand landmarks

```python
# scripts/gesture_events/forcefield_event.py
hand = self._gestures["hand"]["wrist_visible"].get_last_position()
```

Events → define hand and also triggers, same for right hand

```json
// data/events.json
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

```python
# scripts/gesture_events/forcefield_event.py
def update(self):
    if self._state:
        hand = self._gestures["hand"]["wrist_visible"].get_last_position()
        
        hand_landmark = hand.get_landmark("index_tip")
        palm_height = hand.get_palm_height()
        
        depth = palm_height
        normalised_depth = self._normalise_depth(depth)
        self._get_hand_depth_level(normalised_depth)
        
        # debug info
        # print(f'level: {self._current_level} | depth: {depth} | buttons: {self._buttons}')
        
        # add/remove from buffer
        self._depth_buffer.append(self._current_level)
        if self._depth_buffer.count(self._current_level) < self._depth_buffer_size:
            # not all items in buffer are same as current level
            return
        
        # print(f'current: {self._current_level} | triggered: {self._triggered_level}')
        if self._current_level > 0:
            # press key for this level boundary
            for index, button in enumerate(self._buttons):
                level = index + 1
                if level == self._current_level:
                    # must be going forwards
                    if button[1] is False and self._current_level > self._triggered_level: 
                        self._trigger_button(button)
                        self._triggered_level = level
                        
                        print(f'*TRIGGER* level: {level}')
                elif button[1] is True:
                    print(f'*RELEASE* level: {level}')
                    self._release_button(button)
        elif self._current_level == 0:
            # release all keys
            for button in self._buttons:
                if self._triggered_level > 0:
                    self._triggered_level = 0
                if button[1] is True:
                    print('*TRIGGER* level: 0')
        
                    self._release_button(button)
```

Palm height doesn't work well for contractures hands can be rotated, depth z

MediaPipe Depth value

Wrist visible gesture

Dynamically calculate thresholds

### Punching

![wiiu_wiisportsclub_scrn02_e3-2.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8cb5258-daef-480a-a711-9d21a7bb3aa6/wiiu_wiisportsclub_scrn02_e3-2.jpg)

Fist gesture

```json
"fist": {
    "index_folded": true,
    "middle_folded": true,
    "ring_folded": true,
    "pinky_folded": true
},
```

Knuckle height primitive

```python
def get_knuckle_height(self) -> Optional[float]:
		"""Returns the distance between the knuckles"""
		# TODO am i repeatedly recalculating it.
		if "pinky_base" not in self._landmarks or "index_upperj" not in self._landmarks: return None
		return self.get_landmarks_distance("pinky_base", "index_upperj")
```

Events → define hand and also triggers, same for right hand

```json
"hand_punch_left_hand": {
    "type": "PunchEvent",
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

Calculate knuckle height, closer to the camera increases

```python
def _get_hand_depth_level(self, hand_depth):
    """gets the current hand depth level comparing it to the boundaries"""

    if hand_depth > self._depth_boundaries[-1]:
        self._current_level = self._num_buttons
        return

    for index, boundary in enumerate(self._depth_boundaries):
        if hand_depth < boundary:
            self._current_level = index
            break
```

```python
# store buffer of depth values at each frame
self._depth_buffer_size = config.get_data("events/contractures/buttons_activation_time")
self._depth_buffer = deque(maxlen=self._depth_buffer_size)
```

```python
# add/remove from buffer
self._depth_buffer.append(self._current_level)
if self._depth_buffer.count(self._current_level) < self._depth_buffer_size:
    # not all items in buffer are same as current level
    return
```

## Unity Game

- animals walking towards the camera