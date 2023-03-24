---
permalink: /requirements/
title: "Requirements"
excerpt: ""
last_modified_at: 
toc: true
---

## Project Background

A report, published by WHO on 2nd December 2022, says that approximately one sixth of people worldwide have significant disability.[1] Simultaneously, people with disabilities have higher chance of getting mental health problems such as depression, anxiety, phobias, etc. 

The concept of VR therapy was formally introduced in 1990’s and early 2000’s. It first appears applicable in the treatment of acrophobia and it gradually made its way into the disability therapy industry which patients are more acceptable than traditional treatments[2] like rehabilitation training, medication, medical attention.[3]

With this in mind, and after communicating with VR Therapies and and understanding our users’ difficulties, special needs and interests, we decided to develop “Nature Adventure”, a pseudo-VR sensory game which allows users to be immersed in the world of nature. 

With the help of MotionInput and Google MediaPipe, we will be developing 3 gestures: forcefield,  wrist tracking and facial gesture for interaction and navigation with Unity scenes. There will be 4 scenes including Underwater scene, Forest scene, Snowy Mountain scene, Beach scene to enable people with disabilities to go on a virtual walk and interact with the animals. 

## Partner Introduction

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/Untitled.jpeg)

Our partner company Virtual Reality Therapies, a Community Interest Company(CIC) established in 2018, is the first social enterprise in Northampton that gives different VR therapies access to all people with  disabilities, such as wheelchair user, autistic child, people with ALS or MND, etc.[4]


![alt]({{ site.url }}{{ site.baseurl }}/assets/images/Untitled%201.jpeg)

They provide many technical supports for a variety of immersive treatments, including many VR games, activity room, wheelchair-accessible VR driving simulators, interactive sensory room, hydrotherapy with underwater VR headsets, etc, to keep patients entertained with multi-sensory experiences to improve their condition. [4]

![alt]({{ site.url }}{{ site.baseurl }}/assets/images/Untitled%202.jpeg)

Our client, Rebecca Gill, who is the founder of VR therapies, is experienced in learning disabilities, brain injury rehabilitation, mental health issues and other neurological disorders for over 12 years. She has created innovative therapies, underwater VR scenes for aquatic rehabilitation and is leading the way for accessible VR within the local community. 

Here is their official website address: [https://vrtherapies.co.uk](https://vrtherapies.co.uk/)

## Project Goals

- User experience goals:
    - Let them interact with elments of the real world
    - Explore nature in an interactive way as going outdoor
    - Feel engaging and increase their attention span
    - Motivate them to move their body up
    - Improve their mental health issues (e.g. depression, stress)
    - Give them the sense of controlling something and being independent
- Usability goals:
    - Effectiveness - friendly design for people with disabilities of all ages
    - Efficiency - have few steps and less options while playing
    - Safety - clear description of error and guide to correct the error
    - Utility - making all objects and background interactive and realistic
    - Learnability and memorability - have short and clear guide or animation, little to no text

## Requirement gathering

- **Discovering requirements:**
    - Brainstorming :
        - Identify the relevant stakeholders
        - Establish project goals and objectives
        - Document all the ideas
    - Data gathering:
        - Conduct structured interview with NHS GP who works with special needs children
            
            [Interview](https://www.notion.so/Interview-a1a542d48f7340a2ab824770d9f50546)
            
        - Have several meeting with our client(Rebecca) who is specialized in taking care of special needs people
    - Reviews of similar products:
        - ABZU
        - Kinectimals
        - Nature Treks
        - Ocean Rift
        - Here is the link introducing all the games: [spreadsheet]
- **Analyze data:** We didn’t use survey to collect data, since there is privacy and accessibility issues to contact users with disabilities. Therefore we use alternatives mentioned above.

## Personas and Scenarios

### Persona 1
    
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/Screen_Shot_2022-12-23_at_9.13.31_PM.png)

James was a very quiet child most of the time and he was afraid of dealing with strangers, even children at the same age, so he was often left alone. His grandmother looks after him when both of his parents went to work. He often mumbles when doing things that interest him, but over the past few months, grandma noticed that he mumbles less when he is reading storybooks and gets bored quickly. His attention is on the moving objects on his iPad screen and his puppy, Jacky.
    
### Persona 2
    
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/Screen_Shot_2022-12-23_at_9.14.26_PM.png)

Chloe was born with ALS, and she is unable to walk. With the help of a wheelchair, she can move around to different places. Due to her own circumstances, Chloe is interested in biology, and sheis an animal lover who enjoys observing animals and takes care of insects at home. She enjoys using social media and occasionally plays video games. Recently, she was introduced to VR therapy, and she found it amazing to interact with different objects and being able to walk around in the game.
    

## Use cases

### Use case diagram
    
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/IMG_5A7A58D9E6B4-1.jpeg)
    
### Use cases list
    
![alt]({{ site.url }}{{ site.baseurl }}/assets/images/Screen_Shot_2023-03-17_at_4.40.09_PM.png)
    

## MoSCoW Requirements

### Functional

| ID | Functional Requirements for MFC | Priority |
| --- | --- | --- |
| 1 | MFC MUST provide Therapist with different navigation method to select for users: wrist tracking, punch | Must have |
| 2 | MFC MUST provide adjustment bar for different navigation | Must have |
| 3 | The navigation method MUST be capable of controlling the entire game | Must have |
| 4 | The navigation method SHOULD consider Noise Reduction for people with cerebral palsy  | Should have |
| 5 | MFC COULD involve voice recognization to have speech hotkeys | Could have |
| ID | Functional Requirements for Unity Game | Priority |
| 1 | The game MUST provide a main menu and pause menu for for user to manage the game | Must have |
| 2 | The game MUST have different scenes for user to select: underwater, forest, snowy mountain, beach | Must have |
| 3 | The game MUST have a cursor to locate user's navigation while controlling the game | Must have |
| 4 | The game MUST allow users to have 360 panoramic views | Must have |
| 5 | The game MUST generate environment and many different animals | Must have |
| 6 | The animals MUST perform interactions like: animals coming towards player, eating, etc. | Must have |
| 7 | The animals MUST perform many different random actions when users are not interacting with them | Must have |
| 8 | The game SHOULD involve background music or noise of nature while playing | Should have |
| 9 | The environment SHOULD be dynamic by default | Should have |
| 10 | The game SHOULD involve voice recognition to recognize hot keys | Should have |
| 11 | The animals COULD make noise in the game  | Could have |
| 12 | The game COULD add time and weather variation | Could have |

### Non-Functional

| ID  | Non-functional Requirements of MFC | Priority |
| --- | --- | --- |
| 1 | The MFC MUST have simple design to let user select navigation method and adjust it (Learnability) | Must have |
| 2 | The MFC MUST take short response time to recognize user input and  start selected navigation (Performance) | Must have |
| 3 | The navigation MUST be precise, responsive to users with or without cerebral palsy (Satisfaction) | Must have |
| 4 | The error rate of navigation MUST not exceed 10 percent (Errors) | Must have |
| 5 | The gMFC MUST support Window systems and their current version (Compatibility) | Must have |
| 6 | The MFC COULD have different language and text format according to different region (Localization) | Could have |
| 7 | The MFC COULD support other systems and their older version (Compatibility) | Could have |
| ID | Non-functional Requirements of Unity Game | Priority |
| 1 | The game MUST take short response time to generate every element or make element interact with player (Performance) | Must have |
| 2 | The game MUST fit to different devices: PC, TV, projectors and waterproof equipment with a camera, without any change in its behavior and performance (Portability) | Must have |
| 3 | The game MUST support Window systems and their current version (Compatibility) | Must have |
| 4 | The error rate of game response to users' selection in main menu, scene selection menu, pause menu MUST not exceed 10 percent (Errors) | Must have |
| 5 | The game MUST have simple design to let user to take short time to do action (Learnability) | Must have |
| 6 | The game SHOULD have friendly design throughout *please refer to the last question in the interview (Satisfaction) | Should have |
| 7 | The game SHOULD display fluent animation (Satisfaction) | Should have |
| 8 | The game SHOULD be available to worldwide users 24/7 (Availability) | Should have |
| 9 | The system SHOULD perform no failure of use cases (Reliability) | Should have |
| 10 | The system COULD have different language and text format according to different region(Localization) | Could have |
| 11 | The game COULD support other systems and their older version (Compatibility) | Could have |

## References

[1] [https://www.who.int/news-room/fact-sheets/detail/disability-and-health](https://www.who.int/news-room/fact-sheets/detail/disability-and-health)

[2] Maples-Keller JL, Bunnell BE, Kim SJ, Rothbaum BO. The Use of Virtual Reality Technology in the Treatment of Anxiety and Other Psychiatric Disorders. Harv Rev Psychiatry. 2017 May/Jun;25(3):103-113. doi: 10.1097/HRP.0000000000000138. PMID: 28475502; PMCID: PMC5421394.

[3] Center for Substance Abuse Treatment. Substance Use Disorder Treatment For People With Physical and Cognitive Disabilities. Rockville (MD): Substance Abuse and Mental Health Services Administration (US); 1998. (Treatment Improvement Protocol (TIP) Series, No. 29.) Chapter 3—Treatment Planning and Service Delivery. Available from: https://www.ncbi.nlm.nih.gov/books/NBK64875/

[4] [https://vrtherapies.co.uk](https://vrtherapies.co.uk/)