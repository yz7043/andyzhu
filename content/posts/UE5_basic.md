---
title: "UE5_basic"
date: 2023-05-01T11:56:32-04:00
draft: false
---

# Introduction

I am learning UE5 with C++. Here is my notes about some fundamental miscellaneous in UE5.

# Outlines

[Axis Map]({{< ref "UE5_basic.md#AxisMap" >}})

[Root Component]({{< ref "UE5_basic.md#RootComponent" >}})

[Auto Possess]({{< ref "UE5_basic.md#AutoPossess" >}})

[Pawn Movement]({{< ref "UE5_basic.md#PawnMovement" >}})

[Character Movement]({{< ref "UE5_basic.md#CharacterMovement" >}})

# Content

## Axis Map{#AxisMap}

1. Add the mapping to project settings so that our input can be caught and the callback function can be executed every frame

   1. Inside `project setting` we open `Input`tab.
   2. Inside `Input`, we choose `Bindings`
   3. We can add one function name, for example, `MoveForward`
   4. Then we need to add a binded key/keys to this function, for example, 'W' key.

2. We need to add corresponding functions in our PAWN class

   1. In the header file

      ```C++
      void MoveForward(float Val);
      ```

   2. In the Cpp file

      ```c++
      void MyPawn::MoveForward(float Val){
      	/*Detail codes*/
      }
      
      void MyPawn::SetupPlayerInputComponent(UInputComponent* PlayerInputCmp){
      	// This a virtual function created by Pawn class
      	// It is used to setup user input binding
      	// Param: AxisName, Object Ptr, Callback Function Ptr
      	PlayerInputCmp->BindAxis(FName("MoveForward"), this, &MyPawn::MoveForward);
      }
      ```

      

## How to get root component {#RootComponent}

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

## What is auto possess of a PAWN class{#AutoPossess}
1. You can do it in the UE editor
    When we place one pawn blueprint in our world, we are able to control this pawn as our player. How do we configure this?
    1. First we need to select the pawn object from outliner
    2. You may find a `Details` tab sitting besides `World Settings`. Click that `Details` tab.
    3. Then search for `auto possess` and change the `auto possess player` to your player
2. You can also do it in your C++ code
    ```C++
    AutoPossessPlayer = EAutoReceiveInput::Player0;
    ```

## Implement Pawn Movement {#PawnMovement}

1. Move Forward

   ```c++
   void MyPawn::MoveForward(float Value){
     // This function can handle move forward or backward based on whether Value > 0
     // Check if Controller is valid and Value > 0
   	// Controller is an intrinsic variable for Pawn class
   	if((Controller != nullptr) && Value != 0.0){
       // Get Forward direction
       FVector FW = GetActorForwardVector();
       AddMovementInput(FW, Value);
     }
   }
   ```

2. Add movement component in our Pawn Blueprint
   1. Go to MyPawn blueprint
   2. Click + button located at the upper left corner and add a `FloatingPawnMovement`

## Character Movement {#CharacterMovement}

Character class Movement is similar to our Pawn class. 

Here we wish to introduce more challenging task.

1. We want to move towards the direction our controller are facing not the instead of the direction of Mesh
2. We want that the character mesh rotate towards to the direction we are moving
3. When I move the mouse, I expect the camera move according to the controller not the character (To avoid character facing down to the ground which is against nature)

For the first challenge, we need to make use of the Rotation Matrix of Controller instead of `GetActorForwardVector`

```C++
void MoveForward(float V){
    if(Contoller && V != 0.f){
        // Get the Rotator of controller
        FRotator ControllerRotation = GetControllerRotation();
        // Because we are moving horizontally, we only care Yaw rotation
    	FRotator YawRotation(0.f, ControllerRotation, 0.f);
        // Get the "forward" direction by rotation matrix
        // EAxis::X -> Move forward or back based on V
        // EAxis::Y -> Move left or right
        FVector Direction = FRotationMatrix(YawRotation).GetAxis(EAxis::X);
    	AddMovementInput(Direction, V);
    }
}
```

Then, how do we enable the character to rotate towards where it is moving?

```C++
GetCharacterMovement()->bOrientRotationToMovement = true;
```

Third task.

Check `Use Pawn Control Rotation` for the boom