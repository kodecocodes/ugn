```metadata
number: "4"
title: "Chapter 4: Inputs and Movement"
```

# Chapter 4: Inputs and Movement

In the last chapter, you created the player Blueprint, but it didn’t have a way to move or respond to user input. In this chapter, you’ll learn how to set up keyboard and controller inputs and implement movement, starting with the inputs.

> **Note**: This chapter’s project continues from the previous chapter, Chapter 3, "Blueprints." If you didn’t follow along, or you want to start fresh, please use the starter project from this chapter.

## Setting up inputs

First, consider how a beginner might want to set up inputs. Suppose you have a controllable character and you want it to shoot whenever the player left-clicks. Fortunately, this is easy to set up in Blueprints. You create an event for your desired key and connect your shoot logic.

![width=80%](images/t01.png)

Seems easy, right? Now let’s you want another controllable character that can also shoot. No problem, simply add the same event node and connect the shoot logic. One week later, you discover that players love the character variety, so you decide to add another 100 characters — all of whom need to have the ability to shoot. Again, this isn’t a problem because you’ve already mastered the skill of using the Left Mouse Button event.

A few weeks later, players ask that you implement right-click to shoot instead. You sigh, and then proceed to go into all 102 Blueprints and change each event because that's what the players want (you can see where this is going, right?). You finish and hope that you never have to do that again.

Another week later, your business partner informs you that he put together a deal for publishing your game on consoles, so you need to add controller support for shooting. At this point, you start having a meltdown because the thought of having to go through all 102 Blueprints again is soul-crushing.

While this might be an extreme example, it’s a demonstration of a real programming problem called **hard coding**. Hard coding is when you fix logic or data in a way that makes it difficult to modify later. In the example with the shoot action, the input key was hard coded because you explicitly defined it in each Blueprint, making it tedious to change things.

>**Note**: In this book, you’ll try to avoid hard coding unless it makes the Blueprint unnecessarily complicated.

One way to prevent hard coding inputs in your Blueprints is to use **mappings**.

### Understanding mappings

Let’s say you want to get in shape, so you sign up for a gym membership and hire a personal trainer. To make your training more intense, the trainer decides that you must do 10 jumping jacks every time he says “jump”. Combined, you have an event — the trainer telling you to jump — and an action — you performing the jump.

One day, your trainer shows up at your door, barely able to speak due to a sore throat. Since you already paid for the session, you agree to continue with the jumping routine. However, instead of saying the word “jump”, the trainer holds up a sign with the letters J-U-M-P written in black ink. You do your jumping jacks routine, and call it day.

In this example, the way in which **event** occurs is different: Using a sign vs. giving a spoken command. But that’s OK. You only care _when_ the event occurs rather than _how_ it happens. If you apply this idea to the shooting example, then characters would only care about when the player wants to shoot, not which button causes the action — and this is precisely what mappings allow you to do.

Mappings are a list of actions and their assigned keys, which you define in your project settings. For example, a jump mapping might use the space bar and a right-click. When you create a mapping, all actors have access to an event for that specific mapping, which executes when you press any of its assigned keys. With mappings, actors don’t have any key-specific events in their Blueprint. If they want to know when the player wants to jump, they only need to use the jump mapping’s event. Moreover, if you want to add or change a key, you can easily do so in your project settings instead of having to edit each Blueprint.

Now that you know what mappings are all about, it’s time to create the mappings for movement.

### Creating the movement mappings

First, go to **Edit ▸ Project Settings**. Open the **Engine ▸ Input** page. At the top of this page, you’ll see the **Bindings** section. This is where you’ll create all of your mappings.

The first thing you’ll notice is that there are two types of mappings: **action** and **axis**. Each mapping has an associated event. The difference between action and axis mappings is in the way the events execute. Here’s an example of an action event and an axis event:

![width=80%](images/i01.png)

Action events execute when you **press** or **release** an assigned key. For example, if you press the key for Jump, the InputAction Jump event executes once. Any nodes connected to the **Pressed** pin also executes. Once you release the jump key, the action event executes again, but this time, any nodes connected to the **Released** pin executes. Action mappings are great for actions where you only need to press the key once such as jumping or opening a menu.

Axis events work a litte differently. Instead of executing on a keypress, axis events execute **continuously**. This makes them great for actions where you need to hold a key such as moving or shooting. Since this chapter is about movement, an axis mapping is the way to go here.

To move on a 2D plane, you need to move in four directions: up, down, left and right. Can you guess how many axis mappings you’ll need for this? If you answered four, you’re correct — but you can get away with using only two mappings! 

You’ll create the mappings first and then you’ll see how it works.

### Creating axis mappings

First, create two **axis mappings** by using the **+** button. Rename these to **MoveUp** and **MoveRight**. The first mapping handles moving up and down while the second mapping handles moving left and right.

Up next is to assign keys for each mapping. For this game, **W** and **S** move up and down while **A** and **D** move left and right. You’ll also add controller support by mapping movement to the **left thumbstick**.

Each mapping needs three inputs, so add another two to each mapping by using the **+** button next to the mapping’s name. To assign a key, click on the drop-down and search for the key by name. Set up your mappings like so:

![width=80%](images/i02.png)

>**Note**: You’ll notice that the thumbstick only needs one key, unlike the keyboard keys which need two. I’ll explain why this is in the next section.

Now that you’ve got your mappings set up, it’s time to look at how you can use two mappings to represent four directions. The key to this is the **Scale** field.

### Understanding input scales

Every input produces a numerical value called an **axis value**. Its value is determined by the type of input and how you use it. For example, keyboard keys output 1 when pressed and 0 when not pressed. Thumbsticks output a value between -1 and 1 depending on the direction and how far you push it.

![width=80%](images/t02.png)

Suppose you have a character and you want to move it forward. First, you get the character’s current direction and multiply it by a certain speed. This creates an offset which you can then add to the character’s location to move it.

If you multiply the speed, direction or offset by a negative number before adding, the character moves in the opposite direction instead. Since the thumbstick’s axis value can be negative or positive, you can use it as the multiplier.

![width=80%](images/t03.png)

This method works with thumbsticks automatically since their axis value covers the -1 to 1 range. However, keyboard keys can only output an axis value of 0 or 1. To fix this, you need one key to output 1 and another to output -1. Set the **Scale** of the **S** and **A** keys to **-1**.

![width=80%](images/i03.png)

>**Note**: Action mappings do not have a scale because they only have two states: pressed and released.

Now the **S** and **A** keys output -1 when pressed. This works because the engine multiplies the key’s axis value by its scale.

Next comes the fun part: Making the player move!

## Moving the player

Close the Project Settings and open **BP_Player**.

First, you need to get the events for your movement mappings. Open the context menu and search for **MoveUp**. Add the one listed under **Axis Events**, not the green one under Axis Values. Repeat the process to add the **MoveRight** event.

![width=80%](images/i04.png)

In the previous section, you learned that you could move a character by adding an offset to its location. For example, to move up, the offset is a vector pointing up multiplied by a speed. Here’s what that looks like:

![width=80%](images/t04.png)

>**Note**: In this game, the up direction is along the negative Y-axis.

Can you see the problem here? These values are hard coded, and you’ve already learned that hard coding can make your life miserable. A way to solve this is to use **variables**.

### Using variables

Simply put, a variable is something that holds a value. This can be anything from a piece of text to a reference to an actor. What the variable holds depends on its **type**. Here are the most common types and what they hold:

 - **Boolean**: True or false.
 - **Integer**: Whole number, meaning a number without fractions such as 1, 50 and 1000.
 - **Float**: A number with a decimal point such as 0.5, 30.1 and 100.37.
 - **String**: Any piece of text. It can include numbers, letters, spaces and any other character.
 - **3D Vector**: Three-dimensional vector.
 - **2D Vector**: Two-dimensional vector.

For this section, you’ll leave the direction, but you’ll create a variable for the speed. To create a variable, go to the **My Blueprint** tab and click the **+** button to the right of the **Variables** section.

![width=80%](images/i05.png)

With your new variable selected, go to the **Details** tab and rename it to **Speed**. Next, you need to change its type. Either an integer or float will work here, but floats are easier to work with because the nodes you need already use floats. Change the **Variable Type** to **Float**.

![width=80%](images/i06.png)

Next, you need to set the default speed. First, click **Compile** to update the Blueprint. Then, go down to the **Default Value** section and change **Speed** to **30**.

Now that you have the speed variable set up, it’s time to implement movement.

### Implementing movement

Locate the **MoveUp** event and create the following nodes:

 - **Get Speed**
 - **float \* float**
 - **vector \* float**
 - **AddActorWorldOffset**

The vector pin of the **vector \* float** node determines in which direction to move. Since the up direction for this game is along the negative Y-axis, set this to **(0, -1, 0)**. Then, connect everything like so:

![width=80%](images/i07.png)

Here’s a summary of what’s happening:

1. The **MoveUp** event continually executes and output an **axis value** that ranges from -1 to 1. Your input type and its scale determine this value.
2. Multiplying **Speed** by the axis value results in either a positive or negative speed. It can also be zero if the player doesn’t press anything.
3. Multiplying the up direction with the previous result creates an offset that points up or down.
4. **AddActorWorldOffset** takes this offset and moves its **Target** by that amount. In this case, nothing is connected to Target, so it defaults to moving the actor that called it.

Repeat the process for **MoveRight**, but change the direction to **(1, 0, 0)** instead. See how much you can do yourself without reviewing the instructions above. :]

![width=80%](images/i08.png)

That’s all you need for movement, so it’s time to test it out. Click **Compile**, and then click **Play**. Right away, you’ll notice that movement doesn’t work. You set up all of the nodes correctly, so what’s going on here?

>**Note**: By default, the editor plays the game in the main viewport. I recommend changing it to play in a new window instead, so you don’t have to switch between editors all of the time. To do this, click **▼** next to the **Play** button and select **New Editor Window (PIE)**.

Remember when creating the player Blueprint, you inherited from **Actor**? Unlike pawns, actors don’t handle inputs by default. To fix this, make sure you’re in **BP_Player** and click **Class Defaults** in the Toolbar.

![width=80%](images/i09.png)

This displays the class-level settings in the Details panel. Go down to the **Input** section and set **Auto Receive Input** to **Player 0**.

![width=80%](images/i10.png)

Now, the player Blueprint automatically handles any inputs coming from the first player. Click **Play** to test out the movement.

![width=80%](images/b01.png)

Movement is working great now, but there’s one problem: If you play the game on a fast computer, the actor moves at a faster rate. Conversely, if you play on a slow computer, the actor moves at a slower rate. This is due to a problem called **frame rate dependence**.

### Understanding framerate dependence

Most games operate in a loop, meaning they run indefinitely. Inside the loop, the game performs a series of tasks in order. A simple example would be running the Blueprint for every actor and then rendering every actor to the screen. Once it finishes rendering, the image you get is called a **frame** and the time it takes to render that frame is called **Delta Time** or **Delta Seconds**. After rendering, the game repeats the tasks.

> **Note**: It’s also common to refer to one loop cycle as a frame.

Unreal executes axis events once per frame. This means more frames per second causes the movement code to execute more often, which causes the player to move faster. In other words, the speed is **dependent** on the frame rate. To fix this, you need to make sure the speed is **independent** of the frame rate by multiply your speed by Delta Seconds.

For example, imagine a character with a maximum speed of 100. If one second passes since the last frame, the character moves the full 100 units. If half a second passes, it moves 50 units.

![width=80%](images/t05.png)

This is a frame rate independent implementation. If it were dependent, the character would move 100 units every frame, regardless of the time between frames.

First, you’ll make the moving up code frame rate independent. Since you need to multiply the speed by Delta Seconds, you need another input on the **float \* float** node. Click **Add pin**.

Now, create a **Get World Delta Seconds** node and connect it to the input you just added. Repeat the process for the moving right code.

![width=80%](images/i11.png)

Doing this makes movement very slow because now you’re only using a fraction of the speed each frame. To fix this, you need to increase the speed value. Change **Speed** to **2000**. Since movement is now framerate independent, this value means the player moves at a rate of 2000 units per second.

**Compile** and then click **Play** to test the game. You won’t notice a difference, but you can now rest assured that players across different machines move at the same rate.

## Key points

 - Avoid **hard coding** where possible as it makes it difficult to modify the code later. This is especially true if you have many classes.
 - **Mappings** allow actors to only care about _when_ an event occurs rather than _how_ it occurs. This also alleviates input hard coding.
 - **Action** events execute when an assigned key is **pressed** or **released**. They are useful for single input actions such as jumping or opening a menu.
 - **Axis** events execute **continuously**, making them useful for actions where you need to hold a key such as moving or shooting.
 - Every input produces a numerical value called an **axis value**. Its value is determined by the type of input and how you use it. For example, keyboard keys output 1 when pressed and 0 when not pressed. Thumbsticks output a value between -1 and 1 depending on the direction and how far you push it.
 - Unreal multiplies a key’s value by its **scale** before outputting it to the mapping’s event. This is useful for inverting a key’s value.
 - **Variables** allow you to store values which you can then use throughout your Blueprint. This is another way to avoid hard coding.
 - **Frame rate independence** is when your logic functions the same across different frame rates.

## Where to go from here?

You should note that the mapping method in this chapter (and throughout the book) is not the most flexible way to set up inputs. Though it removed the hard coding from Blueprints, it’s still a form of hard coding because it doesn’t support the user changing mappings in-game.

To support runtime mapping, you need to create a Blueprint to handle adding and removing mappings at runtime. You also need to make sure you save and load those mappings. You might also need some sort of menu to make it easier for the player to remap keys. If you’d like to learn more about remapping keys at runtime, search for “**ue4 runtime key binding**”. [TODO: Search where?]

In the next chapter, you’ll create a bullet class and implement shooting functionality into the player Blueprint.