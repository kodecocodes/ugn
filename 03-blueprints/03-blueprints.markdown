```metadata
number: "3"
title: "Chapter 3: Blueprints"
```

# Chapter 3: Blueprints

For the longest time, developers would create games using a programming language such as C++ (which is what Unreal Engine uses). This involved learning the language's syntax and then writing lines of code which the computer would then execute. Here's an example of C++ code to move a player forwards and backwards:

```cpp
void APlayer::MoveForward(float Value)
{
	FVector Offset = GetActorForwardVector() * Speed * Value;
	AddActorWorldOffset(Offset, false, nullptr, ETeleportType::None);
}
```

While this is somewhat understandable, the specifics are a mystery if you're not a programmer. To fully understand this code, you would have to dive deeper into C++ which can be an arduous task itself. Luckily, the good folks at Epic Games realized their engine needed to be more accesible so they created the **Blueprint Visual Scripting** system.

The Blueprint Visual Scripting system is a *visual* way of programming, eliminating the need to write code. Like materials, it uses a node-based system which is a much easier format to understand for non-programmers. Here is the Blueprint equivalent of the code from before:

![width=80%](images/t-01.png)

This looks a lot simpler than the C++ code, doesn't it? While Blueprints might seem like the obvious choice for programming, there are times where C++ might be a better option. Here are a few of them:

 - You have a programmer that is already comfortable working with C++.
 - You need to do something computationally heavy like a complex algorithm. C++ code runs faster than Blueprints!
 - You want to modify the engine. You can do this easily with C++ since Epic Games provides the source code for the engine.

With that said, don't feel like your project can only use one method. In fact, it's fairly common for projects to utilize both at the same time. A common workflow is to have the programmer create the base functionality in C++ and then designers can extend from it using Blueprints. This is a good balance between C++'s performance and Blueprint's usability.

In this chapter, you will use Blueprints to create the camera and player. First, let's check out the current state of the camera.

>**Note**: This chapter’s project continues from the previous chapter, Chapter 2, "Materials." If you didn’t follow along, or you want to start fresh, please use the starter project from this chapter.

## Creating the camera

If you click **Play** in the Toolbar, you'll start playing the current level.

![width=80%](images/i-00.png)

Notice that you can move the camera around using the WASD keys and mouse. Unreal sets this up for you by default but since this is a top-down game, a controllable camera is no good. What we want is a static camera that looks down onto the play area.

>**Note**: You can exit from Play mode by pressing **Escape**.

First, you'll need to create a Blueprint for the camera. Navigate to the **Blueprints** folder and click **Add New ▸ Blueprint Class**. This will prompt you to pick a parent class. Just like materials, Blueprints can also be a child of a parent Blueprint. A child Blueprint will inherit the variables (more on variables in the next chapter) and functionality of its parent. Let's look at the first three classes you can inherit from.

The most basic class is **Actor**. As its description states, it is an object that you can place or spawn into your level. _Everything_ you add to the level is a child of Actor, including the player and background meshes you added. The Actor class contains almost no functionality so it's a good class to inherit from if you want to create something from scratch.

The **Pawn** class inherits from the Actor class meaning it is also an actor. It contains extra functionality that makes it easier for a player or artifical intelligence (AI) to take control of it. I'll go into more detail about this process later in the book.

The **Character** class inherits from Pawn meaning it is also an actor. It has the same functionality as Pawn but also includes movement functionality such as running and jumping. This class is useful if you don't want to program movement yourself.

Since the camera exists in the world and doesn't need to be player-controlled, select the **Actor** class and name it **BP_GameCamera**.

![width=80%](images/i-01.png)

Next, double-click on **BP_GameCamera** to open it in the Blueprint editor.

### Navigating the Blueprint editor

Here are the main panels for the Blueprint editor:

![width=80%](images/blueprinteditor.png)

1. **Components**: A list of the current components.
2. **My Blueprint**: Displays lists of your graphs, functions, variables etc.
3. **Event Graph**: This is where you create all your nodes and logic. Pan by holding **right-click** and moving your mouse. Zoom by scrolling your **mouse wheel**.
4. **Details**: Same as the other Details panels. It will display the properties of your currently selected item.

The first task is to create a camera, which you can create by using **components**.

### Creating the camera component

If a Blueprint is a car then components are the parts that make up the car. The doors, wheels and engine are all examples of components.

Components are also not limited to being physical objects. For example, to make a car move, you could add a movement component. You could even make a car fly by adding a flying component. There are many different types of components and their functionality ranges from displaying a mesh to a full-fledged movement system. You can also create your own components using C++ or Blueprints!

To start, let's create a camera component. To do this, go to the Components panel and click **Add Component ▸ Camera**. This will create a new camera component as a child of **DefaultSceneRoot**.

![width=80%](images/i-02.png)

Next, you need to set the camera's location and rotation. Make sure you have the **Camera** component selected and then go to the Details panel. Afterwards, set **Location** to **(0, 0, 1000)** and **Rotation** to **(0, -90, -90)**. This places the camera high enough so that it doesn't accidentally overlap with anything. It also rotates the camera so that it is looking down. You can also check the placement visually by switching to the Viewport tab.

>**Note**: Note that a component's location, rotation and scale are relative to its parent (in this case, DefaultSceneRoot). This means wherever you place BP_GameCamera in the level, the camera will always be 1000 units above that location.

Cameras in Unreal have two projection options: Perspective and orthographic. Perspective is how we see things in the real world. Objects closer to us appear larger than objects that are further away. An orthographic projection removes this perspective distortion, making it great for 2D games.

![width=80%](images/t-02.png)

Since this is essentially a 2D game, orthographic is a good choice here. To use orthographic projection, go to the **Camera Settings** section and set **Projection Mode** to **Orthographic**. While you're here, go ahead and set **Ortho Width** to **4000** as well. This is how wide of an area the camera will capture. Basically, it's the camera's zoom level.

![width=80%](images/i-02-2.png)

Next, you need to tell Unreal to render the game from this camera's point of view. To do this, you'll need to use **nodes**.

### Understanding Blueprint nodes

Similar to material nodes, you can connect Blueprint nodes to other nodes to perform calculations or some sort of function. Almost any action you want a Blueprint to do can be done through nodes.

Blueprint nodes can have special pins called **execution** pins. These are the white triangular pins you'll see on some nodes. A pin on the left is an input and a pin on the right is an output. In order for a node to execute, its input execution pin _must_ have a connection. Here's an example:

![width=80%](images/nodes.png)

Function A and Function B will execute because their input pins have a connection. Function C and Function D will never execute because Function C’s input pin does not have a connection.

Blueprints also have a special type of node known as an **event**. You can identify events by their red color. Here are a few types of events:

![width=80%](images/events.png)

One thing you'll notice is that events do not have an **input** execution pin, which is puzzling because they need one to execute, right? The thing about events is that you execute them by calling them from somewhere else. Here's an example:

![width=80%](images/hello.png)

>**Note**: "Calling" is simply the term used when referring to executing something. For example, if you were to execute a Jump node, you could say you were "calling the Jump function."

In this example, the **PrintHelloWorld** event will execute because **Event BeginPlay** is calling it.

Speaking of Event BeginPlay, let's look at when it's called. Whenever Unreal spawns an actor, it will do some initial setup and then when the actor is ready for gameplay, Unreal will call the actor's BeginPlay event. This event is very useful for performing additional setup or performing something at the start of the level. In this case, it's a good spot to tell the engine to use this camera as soon as possible.

### Using the camera

First, make sure you are in the Event Graph tab and then locate the **Event BeginPlay** node. To make the engine use a specific camera, you need to use a **Set View Target with Blend** node. To create one, **right-click** an empty spot in the graph to bring up the context menu. Next, search for the node by name and then select it. Note that you'll need to uncheck **Context Sensitive** for it to show up.

![width=80%](images/i-05.png)

To use this node, you will need to specify two things. The first is which actor should be the new view point. In this case, it is the current actor. Once specified, Unreal will check if the actor contains a camera component. If it does, Unreal will render the game using the camera's properties such as location, rotation, projection mode etc. If there isn't a camera component, Unreal will use the properties of the root component instead.

The second thing you need to specify is which player should use the new camera. Obviously there is only one player for this game but this is useful if you have a split screen game and only want to change the camera for a specific player.

To reference the current actor, create a **Self** node. Note that this node will show up as **Get a reference to self** in the context menu.

To reference the player, create a **Get Player Controller** node. The **Player Index** determines which player you are referencing. Leave this at **0**.

![width=80%](images/i-04.png)

Afterwards, connect everything as shown below. You can create connections using the same method as material nodes. **Drag-click** a pin to create a wire and then release over another pin to create the connection.

![width=80%](images/i-06.png)

Now when BP_GameCamera spawns, Unreal will immediately use it as its view point.

To make sure the Blueprint uses the change you made, go to the Toolbar and click **Compile**.

![width=80%](images/compile.png)

That's all for the camera so go ahead and close the Blueprint editor. To actually use the camera, you will need to add it to the level. Go to the Content Browser and drag **BP_GameCamera** into the Viewport to instance it. Afterwards, set its **Location** to **(0, 0, 0)** to center it.

Go to the Toolbar and click **Play** to test out the camera. The camera is now top-down and no longer moves through user input.

![width=80%](images/b-01.png)

Now that the camera is done, let's move onto creating the player Blueprint.

## Creating the player

In this chapter, we'll only deal with setting up a collision component and a mesh. The next few chapters will cover implementing movement and shooting.

Let's decide which class the player Blueprint should inherit from. Since this Blueprint needs to be player-controlled, inheriting from Pawn or Character would be the way to go. But for this game, let's have everything inherit from Actor instead. This will make it easier to share functionality between the player, enemies, bullets and power-ups. I'll go into more detail on this in a few chapters.

Create a new Blueprint in the **Blueprints** folder and select **Actor** as the parent class. Rename the Blueprint to **BP_Player** and then open it.

First, you'll need to set up a collision component so the player can collide with other actors. If you've been playing games for a while, you may have heard of the term "hitbox". This refers to an invisible shape the game uses to detect collisions. The shape and complexity of a hitbox varies from genre to genre. For example, in a first-person shooter, character hitboxes are tightly fit to the character model so that shooting feels accurate. On the other hand, a platformer doesn't need that level of accuracy so it would use a much simpler shape such as a capsule.

![width=80%](images/hitbox.png)

For this game, a simple sphere collider is sufficient. To create one, go to the Components panel and create a **Sphere Collision** component. Rename it to **Hitbox**.

As you've probably noticed, Unreal will always create new components as a child of the root component. This is a problem for collisions! When you move an actor in Unreal, the engine will only check collisions for the root component. This is a performance optimization as it is inefficient to check collisions for every component, especially if you have a lot of them.

To ensure collisions work correctly, you need to make the hitbox the root component. To do this, drag the **Hitbox** component over to **DefaultSceneRoot** and then release to make it the new root. This will also delete DefaultSceneRoot.

![width=80%](images/i-07.png)

Up next is adding the player mesh. First, create a **Static Mesh** component and name it **Mesh**. Afterwards, go to the Details panel and set **Static Mesh** to **SM_Player**.

That's it for the components. Click **Compile** and then close the Blueprint editor.

The last step is to replace the player mesh in the level with the Blueprint. Here's an easy way to do it:

1. Select **BP_Player** in the Content Browser.
2. Go to the Viewport and **right-click** on **Player**.
3. From the menu, find the **Replace Selected Actors with** section and select **BP_Player**.

![width=80%](images/replace.png)

## Key points

 - Blueprints are a visual way of programming, making it easier for non-programmers to understand. You can use both C++ and Blueprints in the same project to gain C++'s performance benefits and Blueprint's usability.
 - Anything that exists in the level inherits from the **Actor** class in some way.
 - Blueprints comprise of **components** which add functionality or some sort of visual.
 - Nodes can have **execution** pins, which determine if a node will execute. If a node's **input** execution pin does not have a connection, it will _never_ execute.
 - **Events** do not have input execution pins and only execute when they are called from somewhere else.
 - The **BeginPlay** event executes when Unreal finishes spawning an actor. This event is useful for performing your own additional setup.

In the next chapter, you'll continue building on the player Blueprint by setting up inputs and movement.