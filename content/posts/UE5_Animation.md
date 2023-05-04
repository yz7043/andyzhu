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