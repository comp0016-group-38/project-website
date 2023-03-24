---
title:  "Implementation - Unity"
header:
  teaser: "/assets/images/500x300.png"
categories: 
  - Jekyll
tags:
  - update
---

## Assets

The 3 Unity asset packs we used were “**Snow Forest Pack Cute Series”**, **“Z ! Low Poly Nature Pack”** and **“Low Poly Animated Animals”**.

[insert pics of packs]

## Generating Map

The snowy mountain scene was imported from one of the asset packs. The other 3 scenes were made with the **“Z ! Low Poly Nature Pack”** asset pack. All the animals used were prefabs from **“Low Poly Animated Animals”**.

## Movement

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

## Animations

To change animal animation we change its state. The states are represented by bools and are easy to manipulate. The following code is an example of setting the state of an animal to running.

```csharp
animator.SetBool("isRunning", true);
```

## Interactions

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