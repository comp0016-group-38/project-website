---
permalink: /testing/
title: "Testing"
excerpt: ""
last_modified_at: 
toc: true
---

## Testing Strategy

Testing played a crucial role in our project to ensure that it functioned properly and was free of unexpected errors. Due to the complexity and interdependency of our legacy codebase, we were unable to implement automated unit testing. Instead, we manually conducted both unit and integration testing, running the program numerous times under various conditions. We also sought feedback from individuals who tested our app in different settings and lighting conditions, such as with different levels of arm coverage or people in the background. To gather additional feedback, we showcased our application to a diverse range of audiences.

## Different Conditions

![Conditions]({{ site.url }}{{ site.baseurl }}/assets/images/testing-conditions.png)

Here are some of our findings:

- Doesn't work well in bad lighting, MotionInput doesn't detect hand due to webcam throwing error
- Distance doesn't affect button gestures - as thresholds are calculated dynamically
- Need to be fully in the camera frame
- Sleeves are fully covered - hand landmarks can be inaccurate
- Someone else is in background - may detect other person's hands instead
- Background/clothes similar to skin colour - hands not detected

## User Acceptance Testing

During the final stage of our development process, we performed user acceptance testing on our product by extensively testing it with a range of video games. We made sure to cover a variety of edge cases by testing games which differ slightly in terms of keyboard and mouse usage. Additionally, we also tested the Navigation Mode of our product with various applications such as web browsers, Microsoft Paint, and Google Earth. This allowed us to ensure that our product is able to interact with apps that require both mouse navigation and keypress/mouse actions.

### Simulated testers

We simulated the behaviour of individuals with contractures, by getting our testers to recreate the curved hand shape, limited mobility, and testing fast hand spasm motions.

![Users]({{ site.url }}{{ site.baseurl }}/assets/images/testing-users.png)

### Test Cases

As a result of the game tests, we were able to conclude that our product can support all games that can be played with a gamepad with up to four buttons. By using buttons mode, we can support up to two button triggers for each hand, since there are two levels for forcefields and punching. Through this, we were able to fully simulate the functionalities of 1-4 button gamepads, allowing users to enjoy gameplay using only hand gestures.

### Feedback

**How simple was the application to setup and understand (1-5)?**

- **Average rating:** 4.0
- **Comments:** "At first glance, I was a bit overwhelmed by all the buttons and menus in the MotionInput app. But once I started playing around with it, I was pleasantly surprised by how simple it was to set up and understand. The only thing that required a bit of clarification was the 'spasm reduction' label, but after clicking on the information button, I fully understood what it meant. Overall, I was really impressed with how intuitive and user-friendly the app was."

**How comfortable was the application to use (1-5)?**

- **Average rating:** 4.0
- **Comments:** "The application was perfect for my wrist and hand shape. I did not feel the need to do anything special and that i could get started straight away, this is not usually the case for me. The mouse moved really easily and the fact I could say 'click' made it so much easier. The clicking with my left hand did not always work however, but the voice controls can replace that."

**How quickly did you get used to the application (1-5)?**

- **Average rating:** 4.5
- **Comments:** "It was super quick and easy to get used to the application. The way it tracked my movement was seamless, from the quick mouse response, to the spasm control. I just felt there was nothing that I needed to get used to as it was already done for me."

## Public Tests

We've had the privilege of showcasing our technology to a wide range of audiences, including representatives from Intel, IBM, Sony Playstation, Microsoft Xbox, Great Ormond Street Hospital, and more. Their response has been overwhelmingly positive, recognizing the potential our project has to make a real difference in people's lives.

![Demos]({{ site.url }}{{ site.baseurl }}/assets/images/testing-demos.png)

> The usability of the app was incredible. It was so simple to navigate, and I was able to control a wide variety of applications and games with ease.
> 

> The product idea is great and has the potential to help many people, but it may take some time to adjust to the new system.
> 

> I'm impressed by the impact that MotionInput will have on real people's lives. It's great to see technology being used in such a meaningful way to improve accessibility and quality of life.
>