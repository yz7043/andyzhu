---
title: "UE5_Animation"
date: 2023-05-03T19:57:04-04:00
draft: false
---

# Introduction

I am learning UE5 with C++. Here is my notes about some Animation in UE5.

# Outlines

[Basic Terms]({{<ref "UE5_Animation.md#BasicTerm">}})
[Add Variables To Event Graph]({{<ref "UE5_Animation.md#AddVariables">}})
[Create a C++ Animation Instance]({{<ref "UE5_Animation.md#C++Animation">}})
[State Machine]({{<ref "UE5_Animation.md#StateMachine">}})
[Action Mapping for Jump]({{<ref "UE5_Animation.md#ActionMapping">}})

# Content

## Basic Term{#BasicTerm}

1. `AnimGraph`: Deal with poses
2. `EventGraph`: Flow of execution of logic
3. `StateMachine`: It is a node in `AnimGraph` where we can control how to blend our different movement

## Add Variables{#AddVariables}

![image](https://drive.google.com/uc?export=view&id=14XvOFkDw8FWh-qVjXcvgQoWqqHwpC8m0)
## Create a C++ Animation Instance{#C++Animation}
1. To create a C++ Animation Instance, we need to create a C++ based on `AnimInstance`
2. `virtual void NativeInitializeAnimation()` is the function where we can initialize all kinds of variables for the animation class, for example, an instance reference to a character class
3. `virtual void NativeUpdateAnimation(float DeltaTime)` is the function that will be called every frame to update variables inside this animation class
After doing those things, we just create a `Animation Blueprint` and inherit our C++ class. We will have all the variables we need for our Blueprint.
## State Machine{#StateMachine}
1. It is used to combine a transition among several animations, which you cna modify in our `animGraph`
2. We can have multiple state machine for our `Output Pose`
3. We can cache our state machine by using `Cache Pose`
4. We can use the state machine to chain different animation togethers 
## Action Mapping for Jumping{#ActionMapping}
1. How to do it in Blueprint

  2. First add `ActionMapping` in project settings
  3. Then add `InputAction Jump` Node and `Jump` function node in the Event Graph. Then we just connect them together

4. How to do it in C++

   ```c++
   PlayerInputComponent->BindAction(FName("Jump"), IE_Pressed, this, &ACharacter::Jump);
   ```

5. Then, how to play the falling animation?

   1. In Blue print
      1. We can get our `My Character Movement` and get the result from function `is Falling`
6. What if the landing animation is too long and I wish the character transferring from landing animation to idle animation quickly?

   1. We can use `Get Relevent Anim Time` node to check how long we have played this animation and decide the transition


