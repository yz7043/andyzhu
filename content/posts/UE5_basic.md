---
title: "UE5_basic"
date: 2023-05-01T11:56:32-04:00
draft: False
---

# Introduction

I am learning UE5 with C++. Here is my notes about some fundamental miscellaneous in UE5 that can help us to speed up.

# Content

## 1. How to get root component

Once we have a base component says a **PAWN**. We usually want to add some mesh to this component. How can we do that?

```c++
// Assume we have a MyPawn.h file 
UCLASS()
class LEARN_API AMyPawn : public APawn{
    /* some functions and data members inside this class*/
}

// Then in MyPawn.cpp file
AMyPawn::AMyPawn{
    // Here we want to add a mesh to our capsule
    UCapsuleComponent* Capsule = CreateDefaultSubobject<UCapsuleComponent>(TEXT("Capsule"));
    // We need to do this
    USkeletalMeshComponent* RobotMesh;
    // 1. create the mesh
    RobotMesh = CreateDefaultSubobject<USkeletalMeshComponent>(TEXT("RobotMesh"));
    // 2. Bond Mesh to the root component
    // We can use RootComponent (a protected member here) too.
    // We can use Capsule here since we know it is our root component
    RobotMesh->SetupAttachment(GetRootComponent());
}
```

