---
permalink: /evaluation/
title: "Evaluation"
excerpt: ""
last_modified_at: 
toc: true
---

## Summary of Achievements

### Achievement Table

### Functional

| ID | Functional Requirements for MFC | Priority | State | Contributors |
| --- | --- | --- | --- | --- |
| 1 | MFC MUST provide Therapist with different navigation method to select for users: wrist tracking, punch | Must have | ✅ | Imaad |
| 2 | MFC MUST provide adjustment bar for different navigation | Must have | ✅ | Imaad |
| 3 | The navigation method MUST be capable of controlling the entire game | Must have | ✅ | Imaad |
| 4 | The navigation method SHOULD consider Noise Reduction for people with cerebral palsy  | Should have | ✅ | Imaad |
| 5 | MFC COULD involve voice recognization to have speech hotkeys | Could have | ✅ | Imaad |

| ID | Functional Requirements for Unity Game | Priority | State | Contributors |
| --- | --- | --- | --- | --- |
| 1 | The game MUST provide a main menu and pause menu for for user to manage the game | Must have | ✅ | Jie, Molly |
| 2 | The game MUST have different scenes for user to select: underwater, forest, snowy mountain, beach | Must have | ✅ | Jie, Molly |
| 3 | The game MUST have a cursor to locate user's navigation while controlling the game | Must have | ✅ | Jie, Molly |
| 4 | The game MUST allow users to have 360 panoramic views | Must have | ✅ | Jie, Molly |
| 5 | The game MUST generate environment and many different animals | Must have | ✅ | Jie, Molly |
| 6 | The animals MUST perform interactions like: animals coming towards player, eating, etc. | Must have | ✅ | Jie, Molly |
| 7 | The animals MUST perform different random actions when users are not interacting with them | Must have | ❌ | Jie, Molly |
| 8 | The game SHOULD involve background music or noise of nature while playing | Should have | ✅ | Jie, Molly |
| 9 | The environment SHOULD be dynamic by default | Should have | ❌ | Jie, Molly |
| 10 | The game SHOULD involve voice recognition to recognize hot keys | Should have | ✅ | Jie, Molly |
| 11 | The animals COULD make noise in the game  | Could have | ❌ | Jie, Molly |
| 12 | The game COULD add time and weather variation | Could have | ❌ | Jie, Molly |
|  | Key functionalities (must haves and should haves) | 86% completed |  |  |
|  | Optional functionalities (could haves) | 33% completed |  |  |

### Non-Functional

| ID  | Non-functional Requirements of MFC | Priority | State | Contributors |
| --- | --- | --- | --- | --- |
| 1 | The MFC MUST have simple design to let user select navigation method and adjust it (Learnability) | Must have | ✅ | Imaad |
| 2 | The MFC MUST take short response time to recognize user input and  start selected navigation (Performance) | Must have | ✅ | Imaad |
| 3 | The navigation MUST be precise, responsive to users with or without cerebral palsy (Satisfaction) | Must have | ✅ | Imaad |
| 4 | The error rate of navigation MUST not exceed 10 percent (Errors) | Must have | ✅ | Imaad |
| 5 | The MFC MUST support Window systems and their current version (Compatibility) | Must have | ✅ | Imaad |
| 6 | The MFC COULD have different language and text format according to different region (Localization) | Could have | ❌ | Imaad |
| 7 | The MFC COULD support other systems and their older version (Compatibility) | Could have | ❌ | Imaad |

| ID  | Non-functional Requirements of Unity Game | Priority | State | Contributors |
| --- | --- | --- | --- | --- |
| 1 | The game MUST take short response time to generate every element or make element interact with player (Performance) | Must have | ✅ | Jie, Molly |
| 2 | The game MUST fit to different devices: PC, TV, projectors and waterproof equipment with a camera, without any change in its behavior and performance (Portability) | Must have | ✅ | Jie, Molly |
| 3 | The game MUST support Window systems and their current version (Compatibility) | Must have | ✅ | Jie, Molly |
| 4 | The error rate of game response to users' selection in main menu, scene selection menu, pause menu MUST not exceed 10 percent (Errors) | Must have | ✅ | Jie, Molly |
| 5 | The game MUST have simple design to let user to take short time to do action (Learnability) | Must have | ✅ | Jie, Molly |
| 6 | The game SHOULD have friendly design throughout *please refer to the last question in the interview (Satisfaction) | Should have | ✅ | Jie, Molly |
| 7 | The game SHOULD display fluent animation (Satisfaction) | Should have | ✅ | Jie, Molly |
| 8 | The game SHOULD be available to worldwide users 24/7 (Availability) | Should have | ✅ | Jie, Molly |
| 9 | The system SHOULD perform no failure of use cases (Reliability) | Should have | ✅ | Jie, Molly |
| 10 | The system COULD have different language and text format according to different region (Localization) | Could have | ❌ | Jie, Molly |
| 11 | The game COULD support other systems and their older version (Compatibility) | Could have | ❌ | Jie, Molly |
|  | Key functionalities (must haves and should haves) | 100% completed |  |  |
|  | Optional functionalities (could haves) | 0% completed |  |  |

## Known Bugs

### MotionInput

| ID | Bug | Priority |
| --- | --- | --- |
| 1 | Can’t move mouse in full screen games, need to use win32api | Medium |

### Unity

| ID | Bug | Priority |
| --- | --- | --- |
| 1 | Animal sounds are not registered | Low |
| 2 | Some animals don’t move directly towards the user/camera properly when “j” is pressed, this is due to the problem of asset.  | Low |

## Individual Contribution Distribution Table

| Work packages | Imaad | Molly | Jie | Akram |
| --- | --- | --- | --- | --- |
| Client Liaison | 80% | 20% | 0% | 0% |
| Requirement Analysis | 15% | 60% | 25% | 0% |
| Research | 33% | 33% | 33% | 0% |
| UI Design | 40% | 25% | 35% | 0% |
| Coding | 50% | 25% | 25% | 0% |
| Testing | 33% | 33% | 33% | 0% |
| Report Website | 40% | 40% | 20% | 0% |
| Video Editing | 25% | 70% | 5% | 0% |
| Overall Contribution | 45% | 33% | 22% | 0% |

## Critical Evaluation

### MotionInput MFC

- Functionality - Fully functional with 3 new gestures and multiple modes.
- Stability - Quite stable, usually run at 15-30 FPS and doesn’t crash.
- Efficiency - Quite efficient, as MediaPipe doesn’t take too much computational power.
- Compatibility - Backwards compatible for all versions of Windows and can run on all computers with webcams.
- Maintainability - Documented our changes, commented code, with easy to understand class/function names. Followed the MotionInput repository file structure and design patterns.
- Project management - our performance against objectives and timelines is satisfactory since the materials were given a bit late and requirements are frequently being updated.

### Unity Environment

- Functionality - We have fulfilled all the requirements given to us by our client, in terms of functionality, as we have made it possible for the user to interact with different animals in different surroundings in a nature adventure game.
- Stability - From our testing of the game, we can conclude that our game is quite stable. There are no major issues, and is mostly bug free. Furthermore, the Unity engine is very stable if the game is programmed correctly
- Efficiency - Our game uses low poly assets so it will not be very demanding in regards to the graphics. Therefore, it will run on most hardware
- Compatibility - The Unity game can be build on various Operating Systems such as MacOS, Linux and Windows. However, the fact that MotionInput only runs on Windows limits the scope of this project
- Maintainability - We have refactored the code to be more readable by various methods including removing magic numbers, no repetition and using clear variable names.
- Project management - our performance against objectives and timelines is satisfactory since the materials were given a bit late and requirements are frequently being updated.

## Future Work

In the future, here are some ways in which the project could be improved:

### MotionInput MFC

- Make the calibration more fun and engaging
- Add Hotkey Input instead of dropdown box for setting button actions in MFC
- Ability to add more button levels, as 2 is the current maximum
- Display a graphic overlay to show which level the user is currently in
- Add a joystick mode for wrist-tracking navigation
- Test out different noise reduction algorithms for spasm reduction in wrist-tracking
- Punching - track activation speed for soft/hard punch, instead of just fist height
- MFC - add ability to load/save setting presets for different applications/games
- Integrate speech hotkeys into MFC
- Add spasm detection system → trigger actions

### Unity Environment

- The current code can easily be extended to add more interactions
- The scenes can have dynamic terrain elements that has interactions with the user. For example, flowers can be picked up
- If there are more interactions, then a new settings can be configured in the settings menu to allow the user to select their desired interaction for each button
    - Dolphin playing
    - Waterfall splitting
    - Flower opening
    - Bubbles blowing
    - Rain falling
- The scenes can have dynamic terrain elements that can interact with animals . For example, grass can be pushed aside when an animal walks through it
- Add more sound effects
- Add the ability to allow the player to move in the world
- Dynamically generated map and animals that allows for infinite exploration
- Even greater variety of scenes
- Add levels
- Add reward system