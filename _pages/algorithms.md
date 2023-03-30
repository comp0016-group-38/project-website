---
permalink: /algorithms/
title: "Algorithms"
excerpt: ""
last_modified_at: 
toc: true
---

For all of our gestures using MotionInput, we are utilising the **Google MediaPipe library** which provides an out-of-the-box solution for hand landmark detection. There are 21 landmarks for each hand, as depicted below.

![MediaPipe Hand Landmarks]({{ site.url }}{{ site.baseurl }}/assets/images/mediapipe-hand-landmarks.png)

Throughout this section we’ll reference points shown on this diagram, which you can look back to if needed.  

### Wrist-Tracking

The main idea behind using wrist-tracking is because individuals with contractures may have curved hands, which will render traditional palm tracking inaccurate. Because of this, we track the wrist point instead (Point #0), which is at the base of the hand.

![Wrist hand landmarks]({{ site.url }}{{ site.baseurl }}/assets/images/landmarks-wrist.png)

Since tremors are quite common in indivduals with contractures, we need to account for any hand jittering or spasms and make sure that the mouse movement is smooth. We can do this by performing noise reduction and smoothing on the tracked (x, y) coordinates of the wrist.

Firstly, we perform **Laplacian smoothing** on the tracked coordinate points. Next, we can pass this through a **Gaussian Filter**. Finally, we can add some dampening which applies some extra smoothing. The effects are as shown below:

![Spasm reduction algorithms]({{ site.url }}{{ site.baseurl }}/assets/images/spasm-reduction-algorithms.png)


## Forcefields

The forcefield gesture interaction is one of the innovative features of our application. It allows users to trigger actions simply by passing their hand through an imaginary barrier, or forcefield. During testing, we experimented with three different methods of tracking depth:

1. Measuring palm height (middle finger base #9 - wrist #0)
    
    ```python
    wrist = results.multi_hand_landmarks[0].landmark[mp_hands.HandLandmark.WRIST]
    middle_base = results.multi_hand_landmarks[0].landmark[mp_hands.HandLandmark.MIDDLE_FINGER_MCP]
    palm_height = self.distance_xyz(middle_base, wrist)
    ```
    
2. Measuring side height of hand (pinky base #17 - wrist #0)
    
    ```python
    wrist = results.multi_hand_landmarks[0].landmark[mp_hands.HandLandmark.WRIST]
    pinky_base = results.multi_hand_landmarks[0].landmark[mp_hands.HandLandmark.PINKY_MCP]
    side_height = self.distance_xyz(pinky_base, wrist)
    ```
    
3. Measuring the z coordinate of the pinky base (#17)
    
    ```python
    pinky_base = results.multi_hand_landmarks[0].landmark[mp_hands.HandLandmark.PINKY_MCP]
    pinky_depth = pinky_base.z
    ```
    

![Forcefields hand landmarks]({{ site.url }}{{ site.baseurl }}/assets/images/landmarks-forcefields.png)

After analysing the results, we determined that the side height of the hand is the most effective method of tracking depth. This is because palm height can be inaccurate since the middle finger base isn't always visible, and the z coordinate of the pinky base can be unreliable and jittery. In contrast, the side of the hand is almost always visible, even for individuals with curved hands resulting from conditions like Cerebral Palsy.

## Punching

The other novel interaction we were tasked with was: punching. By detecting when a user is throwing a punch towards the camera, we're able to trigger specific actions or functions. To ensure the accuracy of this gesture, we assume that the user's fist must be clenched for the punch gesture to count.

![Punching hand landmarks]({{ site.url }}{{ site.baseurl }}/assets/images/landmarks-punching.png)

We're able to approximately calculate the height of the user's fist by finding the distance between two landmarks on the knuckles - the pinky base (#17) and the index upper joint (#6). This gives a good indication as it covers the diagonal height of the knuckle area, which will become proportionally larger as the fist gets closer to the camera.

```python
index_upperj = results.multi_hand_landmarks[0].landmark[mp_hands.HandLandmark.INDEX_FINGER_PIP]
pinky_base = results.multi_hand_landmarks[0].landmark[mp_hands.HandLandmark.PINKY_MCP]
fist_height = self.distance_xyz(pinky_base, index_upperj)
```

This allows us to accurately detect the height of the user's punch and use it to trigger specific actions or functions.