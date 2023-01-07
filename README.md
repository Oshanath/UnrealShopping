# UnrealShopping
![Cover](download.jpg)

Unreal Shopping is a virtual fit-on room application created using Unreal Engine 5.

## Problem Domain
Online clothes shopping has certain disadvantages that we are aware of.
* We can choose sizes, but we don't know how exactly it will fit us.
* It looks good on that model, but will it look good on me?
* Sure, we can return them if it does not suit us, but that is just extra work.

__The answer is virtual fit-on rooms. And in this project I will explore some options that will aid the industry in the long run.__

The general approach of a finished product would work in the following general pattern.

* The users can use the application to create a 3D model that resembles the body shapes of themselves.
* The users can choose and apply clothes onto the model to see how they would fit.

__The problem of creating a custom model and clothing it, contains a number of complex subproblems. Since this project is done with the collaboration of LiveRoom, my goal is to create a well-documented, modular and extensible software that encompasses the whole pipeline, however primitive, with room for improvement.__

## Solution

This project will consist of 3 main components,
* Model creation
* Clothing
* Presentation

All 3 of which will be independant from each other. And everything will be built using Unreal Engine 5. (UE5)

### 1. Model creation
This part is reponsible for creating a 3D model with a body shape that is as much similar to the user's body shape as possible.
* In the priliminary phase of the project, I will implement this part of the project to generate a 3D model using the body measurements input by the user. Metahuman will be used possibly.
* Components from both Metahuman and Makehuman are planned to be used.

### 2. Clothing
* This part is responsible for creating and applying clothing onto the 3D model, that is as physics-realistic as possible. 

There are a couple of ways that clothes are simulated.

* The most simple way is to "clothes-paint" the clothes onto the model. This results in a simple illusion of wearing clothes, provided that the clothes of a tight fitting type. For looser clothes like coats, dresses and ties, this looks heavilty unrealistic.

* The next way is to paint clothes but simulate the looser parts of the outfit dynamically. This provides simplicity as well as a touch of realism. Ideal for games.

* The final method, which is also the method I plan to use for this project, is creating clothes as a separate physics object and simulate it using Unreal Engine's built-in clothing simulation. It's a variation of the particle system where they divide a surface into many smaller parts to generate the physics of a fabric.

* Unreal also has a clothing simulator called "chaos" and it is mostly used for simulating dangling objects like the ends of a scarf.

* Marvelous Designer is a professional software that designers use to design 3D models of clothes. It also comprises of a simulator that is highly accurate in generating cloth dynamics.

* I have to make the collision mesh of the model the same as the static mesh.

* Clothes in Unreal Engine are nothing more that 2D planes that have divisions and simulations applied to it.

* We have to make sure the engine knows that it is a plane, because the internal renderer does not render planes that faces away from us. (Optimization of processing power)

* TODO: I have to figure out a way to  clothe the character. Because the character's initial position and the initial position of the clothes will be different. 

### 3. Presentation
* This part is responsible for viewing the created and clothed model in different environments and lighting conditions. This part is aimed to be as customizable as possible for the user.

# Time plan
The weeks are numbered relative to the semester.

* Week 3 : Researching on the topic and Presenting the general UI design in UE5.
* Weeks 4 - 7 : Implementing the model creation
* Weeks 8 - 11 : Implementing clothing system
* Weeks 12 - 14 : Implementing the presentation component of the software

# Clothes simulation

There are a number of methods that are incorporated by engineers when simulating clothes. The 2 fundamental types of techniques are
* Geometric techniques
* Physical techniques

Geometric techniques are only used when properties of clothes do not matter and only a single believable image of a cloth is needed. Since this project focuses on simulations, we will not pursue these techniques further.

The other type of techniques are physical techniques. These try to simulate clothes' physical properties so that they behave like real clothes in dynamic and unpredictable environments.

## Physical techniues

### C. Feynman
This method simulates cloth as a 2D grid in 3D space. Each point in the cloth grid has 3 energies associated with it. Gravitational energy, bending energy, and elasticity energy. Out of these 3, only the gravitational energy is inflicted upon itself, while the other 2 energies are provided by the surrounding points.

Feynman observed that the final position of a cloth is such that all energies of all points in the cloth grid is minimal. So, in order to find the future configurations of the cloth, we can use gradient descent where we descent into lower energies until we find a minimum.

### David Breen's Particle model
One of the best ways to model cloth. Particles are generated where warp (vertical) and weft (horizontal) threads meet. And then just like Feynman's method we defin some energies for the particles and minimize them using mathematical techniques. In this method we come across,

* Stretching energy - tensile strain
* bending energy - Threads bending out of the cloth's plane
* Trellising (shear) energy - Bending around a thread in the cloth's plane
* Gravitational energy - by the particle's mass
* Repelling energy - Artifical energy to keep any 2 particles seperate.

The sum of all of the above is the total energy and the cloth tries to minimize the total energy.

# Implementing in Unreal engine

Unreal engine has already has cloth simulation implemented, but it is heavily limited due to it being a game engine, and optimization is of highest import.

* Currently any skeletal mesh with enough vertices can be made into a cloth in Unreal Engine. 10000 vertices is a good number for a 2m x 2m cloth.

* Cloth collision is heavily limited. Collision is only allowed with static meshes with complex collision disabled, and Physics assets of the skeletal mesh the cloth itself belongs to. So we can't make it collide with another seperate skeletal mesh.

## Settings

* Density
* Bending stiffness
* Collision thickness
* Friction coefficient
* Damping coefficient
* Drag
* Lift
* Iteration count - The number of solver iterations.  This will increase the perceived stiffness
* Maximum iteration count - The maximum number of solver iterations when the frame rate is lower than 60fps.
* Subdivision count - The number of solver substeps.  This will increase the precision of the collisions.

# References

* [Unreal Engine 5](https://www.unrealengine.com/en-US/unreal-engine-5)
* [Metahuman](https://metahuman.unrealengine.com/)
* [MakeHuman](http://www.makehumancommunity.org/)
* [Cloth modelling](https://web.archive.org/web/20070211131507/http://davis.wpi.edu/~matt/courses/cloth/)