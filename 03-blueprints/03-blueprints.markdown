```metadata
number: "3"
title: "Chapter 3: Blueprints"
```

# Chapter 3: Blueprints

Historically, developers used a programming language such as C++, the same language Unreal Engine uses, to make games. This meant learning the language’s syntax and then writing code. From there, the computer would interpret this code and execute things accordingly. Here’s an example of some C++ code that moves a player forward and backward:

```cpp
void APlayer::MoveForward(float Value)
{
    FVector Offset = GetActorForwardVector() * Speed * Value;
    AddActorWorldOffset(Offset, false, nullptr, ETeleportType::None);
}
```

While this is somewhat understandable, the specifics are a mystery if you’re not a C++ programmer. To fully understand this code, you need to dive deeper into the C++ language which is an arduous task itself. Luckily, the folks at Epic Games came up with an alternative solution: the **Blueprint Visual Scripting** system.

The Blueprint Visual Scripting system is a *visual* way to program, and it eliminates the need to write code. Like materials, it uses a node-based system that’s easier to understand — especially for non-programmers. Here’s the Blueprint equivalent for the player movement code you saw earlier:

![width=80%](images/t-01.png)

While using Blueprints for everything might seem like the easiest thing to do, there are times when C++ is the better option. For example:

 - You’re already comfortable working with C++.
 - You need to do something computationally-heavy like a complex algorithm, in which case, C++ code runs faster than Blueprints.
 - You want to modify the engine using C++, which is possible since Epic Games provides the source code for their engine.

But don’t feel like your project has to use one or the other. It’s relatively common for projects to utilize both at the same time. For instance, you might have a programmer create the base functionality in C++ and let the designer extend things using Blueprints. Handling things this way strikes a nice balance between C++’s performance and Blueprint’s usability.

In this chapter, you’ll use Blueprints to create the camera and the player.

> **Note**: This chapter’s starter project continues from the previous chapter’s final project. If you didn’t follow along, or you want to start fresh, you can use the starter project from this chapter.

## Creating the camera

Click **Play** in the Toolbar to start playing the current level.

![width=80%](images/i-00.png)

Move the camera around using the WASD keys and mouse. By default, Unreal sets up these movement controls for you. However, because this is a top-down game and having a controllable camera doesn’t work well for this style, you’ll use a static camera, instead, that looks down at the play area.

> **Note**: Press **Escape** [ TODO: Is this the same as **esc**? ] to exit the Play mode.

First, you need to create a Blueprint for the camera. Navigate to the **Blueprints** folder and click **Add New ▸ Blueprint Class**. This prompts you to select a parent class. 

Like materials, you can make Blueprints a child of a parent Blueprint. Child Blueprints inherit their variables and functionality from their parent. There are three classes from which you can inherit.

The most basic class is **Actor**. Use the Actor class to create an object that you can place or spawn into your level. Everything you add to the level is a child of Actor, including the player and background meshes you added earlier. This class contains almost no functionality, so it’s an excellent choice if you want to create something from scratch.

The **Pawn** class inherits from Actor and contains all of the variables and functionality from Actor. It also contains extra functionality that makes it easier for a player or artificial intelligence (AI) to control it.

The **Character** class inherits from Pawn and contains the variables and functionality of both Actor and Pawn. In addition, it includes movement functionality such as running and jumping. This class is useful when you don’t want to program movement yourself.

Since the camera exists in the world, and the player doesn’t need to control it, you’ll create it using using the basic Actor class. Click **Actor** and name it **BP_GameCamera**.

![width=80%](images/i-01.png)

Next, double-click **BP_GameCamera** to open it in the Blueprint editor.

### Navigating the Blueprint editor

There are four main panels for the Blueprint editor:

![width=80%](images/blueprinteditor.png)

1. **Components**: A list of the current components.
2. **My Blueprint**: Displays lists of your graphs, functions, variables and other items. [ TODO: Items? Is that the correct term? Use anything but "etc".]
3. **Event Graph**: This is where you create your nodes and logic. You can pan by pressing **right-click** and moving your mouse. To zoom, scroll the **mouse wheel**.
4. **Details**: This is the same as the other Details panels. [TODO: What other details panel?] It displays the properties of the currently selected item.

The first task is to make a camera using **components**.

### Creating the camera component

If you’re new to the idea of components, think of them like this: If a Blueprint is a car, then the components are the parts that make up that car. The doors, wheels and engine are all examples of components.

Components are not limited to physical objects; you can use them for other things too. For example, to make a car move, you can add a movement component. To make it fly, you can add a flying component. There are many different types of components, and their functionality range from displaying a mesh to a full-fledged movement system. You can also create your own components using C++ or Blueprints.

Go to the Components panel and click **Add Component ▸ Camera**. This creates a new camera component as a child of **DefaultSceneRoot**.

![width=80%](images/i-02.png)

Now you need to set the camera’s location and rotation. Make sure you have the **Camera** component selected, then go to the Details panel. Set **Location** to **(0, 0, 1000)** and **Rotation** to **(0, -90, -90)**. This places the camera high enough so it doesn’t accidentally overlap with anything. It also rotates the camera so that it’s looking down at the play area. You can check the placement visually by switching to the Viewport tab.

> **Note**: A component’s location, rotation and scale are relative to its parent — in this case, DefaultSceneRoot. This means wherever you place BP_GameCamera in the level, the camera is always 1000 units above that location.

Cameras in Unreal have two projection options: perspective and orthographic. Perspective is how you see things in the real world. Objects closer to you appear larger than objects that are farther away. An orthographic projection removes this perspective distortion, making it a great choice for 2D games.

![width=80%](images/t-02.png)

Since you’re building a 2D game here, orthographic is an excellent choice. Go to the **Camera Settings** section and set **Projection Mode** to **Orthographic**. While you’re here, set **Ortho Width** to **4000** as well. The Ortho Width is how wide of an area the camera captures. In other words, it’s the camera’s zoom level.

![width=80%](images/i-02-2.png)

Next, you need to tell Unreal to render the game from this camera’s point of view. For this, you’ll use **nodes**.

### Understanding Blueprint nodes

Similar to material nodes, you can connect Blueprint nodes to other nodes in order to perform calculations and other functions. Almost any action you want a Blueprint to do, you can do through nodes.

Blueprint nodes can have special pins called **execution** pins. These are the white triangular pins you might see on some nodes. A pin on the left is an input, and a pin on the right is an output. For a node to execute, its input execution pin must have a connection. Here’s an example:

![width=80%](images/nodes.png)

Function A and Function B executes because their input pins have a connection. Function C and Function D never executes because Function C’s input pin does not have a connection.

Blueprints also have a special type of node known as an **event**. You can identify events by their red color. Here are a few types of events:

![width=80%](images/events.png)

One thing you’ll notice is that events do not have an input execution pin. That’s because you execute them by calling them from somewhere else. Here’s an example:

![width=80%](images/hello.png)

> **Note**: “_Calling_” is the term used when referring to executing something. For example, if you were to execute a Jump node, you could say you were “calling the Jump function”. [TODO: I'm not sure this note is needed.]

In this example, **PrintHelloWorld** executes because **Event BeginPlay** is calling it.

Speaking of Event BeginPlay, whenever Unreal spawns an Actor, it does some initial set up. Then, when the Actor is ready for gameplay, Unreal calls the Actor’s BeginPlay event. This event is great for performing additional set up or performing some action at the start of the level. For this game, it’s a great place to tell the engine that it should use the camera you just added.

### Using the camera

First, ensure that you’re in the Event Graph tab, and then locate the **Event BeginPlay** node. To force the engine use a specific camera, you need to use a **Set View Target with Blend** node. *Right-click** an empty spot in the graph to bring up the context menu. Search for the node by name and then select it. 

> **Note**: You’ll need to uncheck **Context Sensitive** for it to show up.

![width=80%](images/i-05.png)

To use this node, you need to specify two things:

1. **Which actor should be the new viewpoint**. In this case, it’s the current Actor. Once specified, Unreal checks if the Actor contains a camera component. If it does, Unreal renders the game using the camera’s properties such as location, rotation and projection mode. If there isn’t a camera component, Unreal uses the properties of the root component instead.

2. **Which player should use the new camera**. There’s only one player for this game, but this is useful if you have a split screen game and want to change only the camera for a specific player.

To reference the current Actor, create a **Self** node. Note that this node shows up as **Get a reference to self** in the context menu.

To reference the player, create a **Get Player Controller** node. The **Player Index** determines which player you’re referencing. Leave this at **0**.

![width=80%](images/i-04.png)

Afterward, connect everything as shown in the following image. You can create connections using the same method as material nodes. **Drag-click** a pin to create a wire and then release it over another pin to create the connection.

![width=80%](images/i-06.png)

Now, when BP_GameCamera spawns, Unreal immediately uses it as its viewpoint.

To ensure the Blueprint uses the change you made, go to the Toolbar and click **Compile**.

![width=80%](images/compile.png)

Close the Blueprint editor because you’re now ready to use the camera. But first, you need to add it to the level. Go to the Content Browser and drag **BP_GameCamera** into the Viewport. Then, set its **Location** to **(0, 0, 0)** to center it.

Go to the Toolbar and click **Play** to test out the camera. The camera is now top-down and no longer moves through user input.

![width=80%](images/b-01.png)

Now that the camera is done, it’ time to move on to creating the player Blueprint.

## Creating the player

In this chapter, you’ll only deal with setting up a collision component and a mesh. The next few chapters [TODO: How many? ] after this will cover implementing movement and shooting.

First, decide from which class the player Blueprint should inherit. Since this Blueprint will be player-controlled, inheriting from Pawn or Character is the better way to go. However, for this game, you’ll have everything inherit from Actor instead because it makes it easier to share functionality between the player, enemies, bullets and power-ups.

Create a new Blueprint in **Blueprints** and select **Actor** as the parent class. Rename the Blueprint to **BP_Player** and then open it.

Next, you need to set up a collision component so the player can collide with other Actors. If you’ve been playing games for any length of time, you may have heard the term **hitbox**. Hitbox refers to an invisible shape the game uses to detect collisions. The shape and complexity of a hitbox varies from genre to genre. For example, in a first-person shooter, character hitboxes are tightly fit to the character model so that shooting feels accurate. On the other hand, a platformer doesn’t need that level of accuracy so it uses a much simpler shape.

![width=80%](images/hitbox.png)

For this game, a simple sphere collider is sufficient. Go to the Components panel and create a **Sphere Collision** component. Rename it to **Hitbox**.

You may have noticed, Unreal always creates a new component as a child of the root component. This is a problem for collisions. When you move an Actor in Unreal, the engine only checks collisions for the root component. This is a performance optimization as it is inefficient to check collisions for every component, especially when you have a lot of them.

To ensure collisions work correctly, you need to make the hitbox the root component. Drag the **Hitbox** component to **DefaultSceneRoot** and then release it to make it the new root. This also deletes DefaultSceneRoot.

![width=80%](images/i-07.png)

Up next is adding the player mesh. First, create a **Static Mesh** component and name it **Mesh**. Then, go to the Details panel and set **Static Mesh** to **SM_Player**.

That’s it for the components. Click **Compile** and then close the Blueprint editor.

The last step is to replace the player mesh in the level with the Blueprint. Here’s an easy way to do it:

1. Select **BP_Player** in the Content Browser.
2. Go to the Viewport and **right-click** on **Player**.
3. From the menu, find the **Replace Selected Actors with** section and select **BP_Player**.

![width=80%](images/replace.png)

[TODO: You need some kind of section wrap-up here.]

## Key points

 - Blueprints are a visual way of programming, making it easier for non-programmers to understand. You can use both C++ and Blueprints in the same project to gain C++’s performance benefits and Blueprint’s usability.
 - Anything that exists in the level inherits from the **Actor** class in some way.
 - Blueprints comprise of **components** which add functionality or visuals.
 - Nodes can have **execution** pins that determine if a node executes. If a node’s **input** execution pin does not have a connection, it never executes.
 - **Events** do not have input execution pins and only execute when they’re called from somewhere else.
 - The **BeginPlay** event executes when Unreal finishes spawning an actor. This event is useful for performing your own additional setup.

In the next chapter, you’ll continue building on the player Blueprint by setting up inputs and movement.