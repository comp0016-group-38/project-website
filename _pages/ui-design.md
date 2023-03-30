---
permalink: /ui-design/
title: "UI Design"
excerpt: ""
last_modified_at: 
toc: true
---

## Config Wizard

### Design Principles

- Simplicity - the design needs to be intuitive, concise and contain the least info needed with all the desired functionality.
- Feedback - lots of iteration and trial/error to make sure it suits the user.

### Wireframe

![MFC Wireframe]({{ site.url }}{{ site.baseurl }}/assets/images/mfc-initial-design.jpg)

Link to [Figma Design](https://www.figma.com/file/4Vqj7ZrC8ubZ92kaC2pjYL/Contractures-MFC?node-id=0%3A1&t=kdPS7Fj8Yagi6ohN-1)

**Feedback:**

- 3 sections allow users to customize - enable/disable
- Simple design and layout
- No guiding information given when selecting
- Not considering users’ preference of main hand
- Repetition for setting keys

### Iterations

We went through a lot of different iterations for the MFC design, as we had to keep on improving it based on the user feedback we received. Here are some images to show how the design changed over time:

![MFC Iteration 1]({{ site.url }}{{ site.baseurl }}/assets/images/mfc-iteration-1.png)

![MFC Iteration 2]({{ site.url }}{{ site.baseurl }}/assets/images/mfc-iteration-2.png)

![MFC Iteration 3]({{ site.url }}{{ site.baseurl }}/assets/images/mfc-iteration-3.png)

### Final Design

![MFC Final Design]({{ site.url }}{{ site.baseurl }}/assets/images/mfc-final-design.jpeg)

This is the final design that we went with. While implementing the actual MFC application, we also realised that there were some restrictions due to limitations and time constraints, so we had to adjust accordingly and do what was feasible. 

**Improvements:**

- More functionalities:
    - More keypress options
    - Can choose main hand
    - Enable/disable speech
    - Turn on/off calibration
    - Gesture delay slider
- Guiding information provided
- Simple design - new grouping, simple layout, have default setting
- Auto-disable keys when different mode is selected

## Unity Game

### Design Principles

- Fun, colorful, child-friendly
- Simplicity - the design should contain the least info needed with all the desired functionality.
- easy to learn - the design should be intuitive to users. e.g. Icons are used instead of words
- Compatibility regarding to users - the design should satisfy their needs of exploring nature and interact with animals.
- Memorability - the design should be easier to remember by using the similar design template of the games on the market. e.g. Start menu includes: Start, Setting and Quit buttons only.
- Responsibility - the system should respond immediately to user’s input

### Sketches

![Game Sketches 1]({{ site.url }}{{ site.baseurl }}/assets/images/unity-sketches-1.png)

![Game Sketches 2]({{ site.url }}{{ site.baseurl }}/assets/images/unity-sketches-2.png)

**Feedback:**

- Similar design template of the games on the market which reduces users learning time
- More graphics and less words
- Idea of using dynamic background instead of still picture - more immersive
- Not colorful - black and white
- Still 2D

### Final Design

**Overall Style:**

- Every element is 3D low poly to target towards children
- Buttons change color when mouse over
- Colorful graphics

**Start Menu:**

- Changed to brighter and colorful background instead of using dark underwater
- New button design is more visually intuitive

![Unity Start Menu]({{ site.url }}{{ site.baseurl }}/assets/images/unity-start-menu.jpg)

**Settings Menu:**

- Has a slider to adjust music and sound effects

![Unity Settings Menu]({{ site.url }}{{ site.baseurl }}/assets/images/unity-settings-menu.jpg)

**Scene Selection Menu:**

- 4 clear options to navigate to the desired scene

![Unity Scene Selection Menu]({{ site.url }}{{ site.baseurl }}/assets/images/unity-scene-selection.jpg)

**Pause Menu:**

- Simple and unobtrusive, with a transparent background

![Unity Pause Menu]({{ site.url }}{{ site.baseurl }}/assets/images/unity-pause-menu.jpg)