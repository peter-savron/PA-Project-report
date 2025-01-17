# Cover Page
# Process Automation PLC-9 Project report
Adam Kern, Peter Savron

FRI – Faculty of Computer Science and Informatics

Mentor: Assist. Prof. dr. Octavian Mihai Machidon

Process Automation - 2024/2025

17/1/2025



## Abstract 

Provide a brief summary (150–200 words) of the project, including objectives, key features, and outcomes.

## Table of Contents

List all sections and subsections with their corresponding page numbers.
Introduction

## 1 Overview of the project
A very well known company, which we cannot name due to signing an NDA, gave us the task of automating their storage facilties by the use of a robotic arm. We were motivated by the promise of a large payout as the project succeeded.

The objective was to create an efficient and user friendly system suitable to their old and untrained workforce for moving large cylindrical containers from the checkout into the storage.

The development had three phases:
* The replacement of manual labor with manually controlled machine labor.
* Automating the machine labor and removing the costly and accident prone manual workforce from the task.
* Creating an user friendly UI for a supervisor to supervise the work of the robotic arm and allow for limited flexibility in its automation.

### 1.1 Description of the Task
As some items are put in the checkout area the appropriate recipe is selected by the supervisor and the machine starts transferring the items to their designated location within the storage facility.
Some requirements had to be met:
* Seamlessly transfer an item from point A to B
* Automate the transfer of items
* Map the storage positions to the controller program
* Track the items position inside the storage facility
* Display the data through an intuitive UI
* Allow for changes in automation to be applied through said UI
Specifications:
* The memory of the controller should allow for tracking of up to 5 items and memorisation of 11 positions in the format: 
```
Position: {
    Rotation: INT;
    Exstension: INT;
    Height: INT;
}
```
* The recipe should have up to 9 stages in the format: 
```
Stage: {
    Item-to-take: INT | NULL;
    Position-to-go: INT | NULL;
    Grab: BOOL;
}
```

### 1.2 Overview of the System
The robot has four movable components:
* A rotating base which rotates up to 270 degrees powered by an electrical motor, with a switch at the start point;
* A screw mechanism moves the upper part of the arm vertically, powered by an electric motor with a switch at the top;
* Another screw which extends and retracts the peripheral part of the robotic arm with a switch at the start point;
* A claw which opens and closes with an circular internal profile with a switch which detects the open position.
* A PLC which controls and synchronises the movement of the motors.
### 1.3 Process Description
We simulated the movement of the robotic arm with a small scale replica which we had in our laboratory which we connected to our PLC.
We used Visual Studio as our IDE with the Twincat code environment and build tools installed.
## 2 Control System Design and Implementation
## 2.1 Control Hardware
The robotic arm has eight output variables:
* Four switches with 0/1 values that signal whether the moving part has reached the initial position.
* Four switches which alternate on and off states as the screw rotates emitting an clock like signal with each upper front of the signalling a quarter of the rotation of the screw.

We can give the robotic arm eight inputs:
* We have four boolean inputs which determine the direction of movement of the motor with true being towards the switch and false being away from it;
* We have four inputs which signal the robotic arm which motor to actuate.
The PLC has six buttons, four lights and a switch.

### 2.2 Control Software
The project was composed of two elements, the project for programming the PLC and a project for programming the HMI interface. We implemented a step be step approach introdusing functionalities one at a time.

### 2.2.1 Manual control
At first we implemented a function block wich was applicable to all the motors. Inside there was the interlock logic, detection of hardware limits in both directions and consequential stopping of the hardware. The function block took as an input the state of the control switch, the input which changes direction of movement, the motor movement clock, the maximum motor extension and whether or not the motor was selected. Inside the motors various criteria were checked, if interlock expired and the motor could change direction, if the motor reached the minimum or maximum of its extension and if motor changed direction so interlock were to be activated. The outputs of the function are three boolean values which control the movement and the direction of the motor and a value which tells whether interlock is active or not, usefull for debbugging and checking motor status.
For manual mode we implemented a multiplexer which choose which motor to activate based on which button of the four black buttons was last pressed. The direction and movement button were common to all the motors, green for the former, red for the latter.

All the implementaions for the manual movement checkpoint were made in structure test.

With the multiplexer and the function block implemented, we met all the requirements for the manual contol checkpoint and received a payout in form of lab assignment points.

### 2.2.2 Automatic control
Next step was to implement a routine which was cyclically executed on the PLC. The routine consists of four separate functions: 
* init();
* automate();
* track();
* move();

#### 2.2.2.1 Init
The init function is responsible for initializing the motor arm when it is in automatic mode and not yet initialized.

#### 2.2.2.2 Automate
The automate function is responsible for reading the instructions in the recept and setting the movement instructions in such a way that it will reach the target location in a safe manner, so retracting first, then rotating, changing height then extending and eventually grabbing or releasing if instructed to do so.

#### 2.2.2.3 Track
The track function is responsible for ttracking the movements of various items in the storage. It detects whether the arm is approching an item, that is if the current instruction is instructing the arm to reach to an item, and writes the item id into the `grabbed` variable once the item is grabbed. While moving it will update the item position in the item table. Once the claw releases the item, grabbed will be set to a `null` value and the program will wait for the next item to be grabbed.

#### 2.2.2.4 Move
It will move the motors and update the arm position. While in manual mode it will work as described above, while in automatic mode it will read the instructions given by the `auto` function and move the arm accordingly.
### 2.3 SCADA Integration (if applicable):
Overview of SCADA features, such as monitoring, control, and alarms.
Include screenshots or illustrations of the SCADA interface.
### 2.4 Implementation Steps:
Summarize the key stages of the control system's development and testing.

## 3 Instructions for Use

### 3.1 System Startup and Shutdown:
Step-by-step guide for turning the system on/off safely.
### 3.2 Manual Mode:
Instructions for manually operating the system.
### 3.3 Automatic Mode:
Explanation of automated processes, including transition triggers.
### 3.4 Alarms and Troubleshooting:
Description of alarm types (e.g., informational, warnings, critical).
Steps to diagnose and resolve common issues.

## 4 Implementation Challenges and Solutions

Describe the main difficulties encountered during the project implementation.
Detail how these challenges were addressed, including:
Technical problems and their solutions.
Insights gained from debugging and testing.
Improvements made to initial designs.
Highlight lessons learned and their relevance to future projects.
Conclusion

Recap the project objectives, challenges, and overall results.
## 5 References

List all sources in a standard citation format (e.g., APA, IEEE).
## 6 Appendices (if necessary)

Include supplementary materials, such as:
## 7 System diagrams.
Code excerpts.
Extended test data or results.
