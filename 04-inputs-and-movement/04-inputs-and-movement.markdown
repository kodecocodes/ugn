```metadata
number: "4"
title: "Chapter 4: Inputs and Movement"
```

# Chapter 4: Inputs and Movement

In the last chapter, you created the player Blueprint but it didn't have a way to move or respond to user input. In this chapter, you will learn how to set up keyboard and controller inputs and implement movement.

Let's start with setting up the inputs.

>**Note**: This chapter’s project continues from the previous chapter, Chapter 3, "Blueprints." If you didn’t follow along, or you want to start fresh, please use the starter project from this chapter.

## Setting up inputs

First, let's look at how a beginner might want to set up inputs. Let's say you have a controllable character and you want them to shoot whenever the player presses left-click. Fortunately, this is very easy to set up in Blueprints. You just create an event for your desired key and then connect your shoot logic.

![width=80%](images/t01.png)

Super easy, right? Now let's say your boss wants another controllable character that can shoot as well. No problem, just slap the same event node in and connect the shoot logic again. One week later, your boss comes back and tells you that players love character variety so the game needs another 100 characters.  Don't forget that they need to be able to shoot as well. Again, this is not a problem because you've already mastered the skill of using the Left Mouse Button event.

Then, one day, your boss tells you he wants shooting to occur on right-click instead. You sigh and then proceed to go into all 102 Blueprints and change each event (you can see where this is going, right?). You finish and pray you never have to do that again.

Another week later, your boss informs you the game got a publishing deal for consoles so you need to add controller support for shooting. At this point, you start having a meltdown because the thought of having to go through all 102 Blueprints again is soul-crushing.

Obviously, this is a bit of an extreme example but it's a demonstration of a real programming problem called **hard coding**. This is when you fix logic or data in a way that makes it difficult to modify later on. In the example above, the input key was hard coded because you explicitly defined it in each Blueprint. Because of this, it became very tedious to add or change keys later on.

>**Note**: In this book, we'll try to avoid hard coding unless it makes the Blueprint unnecessarily complicated.

A way to prevent hard coding inputs in your Blueprints is to use **mappings**.

### Understanding mappings

Let's say you want to get in shape, so you sign up to a gym and hire a personal trainer. To make your training more intense, he decides that you must do 10 jumping jacks every time he says "jump." What you have here is an event (your trainer telling you to jump) and an action (you performing the jump).

One day, your trainer catches a cold so he can't come in. However, you've already paid for the session so he agrees to continue with the jumping routine. His throat is a bit sore though so he decides to text you the word "jump" instead. In this case, the way in which the event occurs is the same. But at the end of the day, you only care about _when_ the event occurs rather than _how_ it occurs. Applying this to the shooting example, characters should only care about _when_ the player wants to shoot rather than what key the player presses. This is what mappings allow you to do.

Mappings are a list of actions and their assigned keys, which you define in your project settings. An example of a mapping would be a jump mapping with space bar and right-click assigned. When you create a mapping, all actors will then have access to an event for that specific mapping, which executes when you press any of its assigned keys. This way, actors don't have any key-specific events in their Blueprint. If they want to know when the player wants to jump, they just need to use the jump mapping's event. And if you want to add or change a key, you can easily do so in your project settings instead of having to edit each Blueprint.

Let's start with creating the mappings for movement.

### Creating the movement mappings

First, go to **Edit ▸ Project Settings**. Afterwards, open the **Engine ▸ Input** page. At the top of this page, you'll see the **Bindings** section. This is where you'll create all your mappings.

The first thing you'll notice is that there are two types of mappings: **Action** and **axis**. As you already know, a mapping will also have an associated event. The difference between action and axis mappings is in the way the events execute. Here is an example of an action event and an axis event:

![width=80%](images/i01.png)

Action events will execute when you **press** or **release** an assigned key. For example, if you press the key for Jump, the InputAction Jump event will execute once. Any nodes connected to the **Pressed** pin will also execute. Once you release the jump key, the action event will execute again but this time, any nodes connected to the **Released** pin will execute. Action mappings are great for any action where you only need to press the key once such as jumping or opening a menu.

Axis events work a bit differently. Instead of executing on a key press, axis events execute **continuously**. This makes them great for actions where you need to hold a key such as moving or shooting. Since this chapter is about movement, an axis mapping is the way to go here.

To move on a 2D plane, you need to be able to move in four directions: Up, down, left and right. Can you guess how many axis mappings you'll need for this? If you answered four, you would be correct but you can actually get away with only using two mappings! Let's create the mappings first and then I'll explain how this works.

### Creating axis mappings

First, create two **axis mappings** by using the **+** button. Rename these to **MoveUp** and **MoveRight**. The first mapping will handle moving up and down while the second mapping will handle moving left and right.

Up next is to assign keys for each mapping. For this game, **W** and **S** will move up and down while **A** and **D** will move left and right. We'll also add controller support by mapping movement to the **left thumbstick**.

Each mapping will need three inputs so add another two to each mapping by using the **+** button next to the mapping's name. To assign a key, click on the drop-down and search for the key by name. Set up your mappings as shown below.

![width=80%](images/i02.png)

>**Note**: You'll notice that the thumbstick only needs one key unlike the keyboard keys which need two. I'll explain why this is in the next section.

Now that you've got your mappings set up, let's look at how you can use two mappings to represent four directions. The key to this is the **Scale** field.

### Understanding input scales

Every input will produce a numerical value called an **axis value**. Its value is determined by the type of input and how you use it. For example, keyboard keys output 1 when pressed and 0 when not pressed. Thumbsticks output a value between -1 and 1 depending on the direction and how far you push it.

![width=80%](images/t02.png)

Let's say you have a character and you want to move it forward. First, you would get the character's current direction and multiply it by a speed. This creates an offset which you can then add to the character's location to move it.

If you multiply the speed, direction or offset by a negative before adding, the character will move in the opposite direction instead. Since the thumbstick's axis value can be negative or positive, you can use it as the multiplier.

![width=80%](images/t03.png)

This method works with thumbsticks automatically since their axis value covers the -1 to 1 range. But keyboard keys can only output an axis value of 0 or 1. To fix this, you need one key to output 1 and another to output -1. To do this, set the **Scale** of the **S** and **A** keys to **-1**.

![width=80%](images/i03.png)

>**Note**: Action mappings do not have a scale because they only have two states: Pressed and released.

Now the S and A keys will output -1 when pressed. This works because the engine multiplies the key's axis value by its scale.

Next comes the fun part: Making the player move!

## Moving the player

Close the Project Settings and then open up **BP_Player**.

First, you need to get the events for your movement mappings. Open the context menu and search for **MoveUp**. Add the one listed under **Axis Events**, not the green one under Axis Values. Afterwards, repeat the process to add the **MoveRight** event.

![width=80%](images/i04.png)

In the last section, you learned that you can move a character by adding an offset to its location. For example, to move up, the offset would be a vector pointing up multiplied by a speed. Here's what that would look like:

![width=80%](images/t04.png)

>**Note**: In this game, the up direction is along the negative Y-axis.

Can you see the problem here? These values are hard coded and we've already learned that hard coding can make your life miserable. A way to solve this is to use **variables**.

### Using variables

Simply put, a variable is something that holds a value. This can be anything from a piece of text to a reference to an actor. What the variable holds depends on its **type**. Here are the most common types and what they hold:

 - **Boolean**: True or false.
 - **Integer**: Whole number meaning a number without fractions such as 1, 50 and 1000.
 - **Float**: A number with a decimal point such as 0.5, 30.1 and 100.37.
 - **String**: Any piece of text. It can include numbers, letters, spaces and any other character.
 - **Vector**: Three-dimensional vector. There is also a Vector 2D type which holds a two-dimensional vector.

For this section, we'll leave the direction hard coded but we'll create a variable for the speed. To create a variable, go to the My Blueprint tab and click the **+** button to the right of the **Variables** section.

![width=80%](images/i05.png)

With your new variable selected, head over to the Details tab and rename it to **Speed**. Next, you need to change its type. Either an integer or float would work here but floats are easier to work with because the nodes you need already use floats. Go ahead and change the **Variable Type** to **Float**.

![width=80%](images/i06.png)

Next, you need to set the default speed. First, click **Compile** to update the Blueprint. Afterwards, go down to the **Default Value** section and change **Speed** to **30**.

Now that you have the speed variable set up, let's implement movement.

### Implementing movement

Locate the **MoveUp** event and then create the following nodes:

 - **Get Speed**
 - **float \* float**
 - **vector \* float**
 - **AddActorWorldOffset**

The vector pin of the **vector \* float** node will determine which direction to move in. Since the up direction for this game is along the negative Y-axis, set this to **(0, -1, 0)**. Afterwards, connect everything like so:

![width=80%](images/i07.png)

Here's a summary of what's happening:

1. The **MoveUp** event will constantly execute and output an **axis value** that ranges from -1 to 1. This value is determined by your input type and its scale.
2. Multiplying **Speed** by the axis value results in either a positive or negative speed. It can also be zero if the player doesn't press anything.
3. Multiplying the up direction with the previous result will create an offset thats points up or down.
4. **AddActorWorldOffset** will take this offset and move its **Target** by that amount. In this case, nothing is connected to Target so it defaults to moving the actor that called it.

Repeat the process for **MoveRight** but change the direction to **(1, 0, 0)** instead. See how much you can do yourself without reviewing the instructions above!

![width=80%](images/i08.png)

That's all you need for movement so let's test it out. Click **Compile** and then click **Play**. Right away, you'll notice that movement doesn't work. You've set up all the nodes correctly so what's going on here?

>**Note**: By default, the editor will play the game in the main viewport. I recommend changing it to play in a new window instead so you don't have to switch between editors all the time. To do this, click the **▼** next to the **Play** button and select **New Editor Window (PIE)**.

Well, remember when creating the player Blueprint, I told you to inherit from **Actor**? Unlike pawns, actors don't handle inputs by default. To fix this, make sure you're in **BP_Player** and then click **Class Defaults** in the Toolbar.

![width=80%](images/i09.png)

This will display the class-level settings in the Details panel. Go down to the **Input** section and set **Auto Receive Input** to **Player 0**.

![width=80%](images/i10.png)

Now, the player Blueprint will automatically handle any inputs coming from the first player. Click **Play** to test out the movement.

![width=80%](images/b01.png)

Movement is working great now but there is actually one problem with it. If you play the game on a fast computer, the actor will actually move at a faster rate. Conversely, if you play on a slow computer, the actor will move at a slower rate. This is due to a problem called **frame rate dependence**.

### Understanding frame rate dependence

Most games operate in a loop, meaning they will run indefinitely. Inside the loop, the game will perform a series of tasks in order. A simple example would be running the Blueprint for every actor and then rendering every actor to the screen. Once it finishes rendering, the image you get is called a **frame** and the time it takes to render that frame is called **Delta Time** or **Delta Seconds**. After rendering, the game repeats the tasks again.

>**Note**: It's also common to refer to one loop cycle as a frame.

Unreal will execute axis events once per frame. This means more frames per second will cause the movement code to execute more often, which causes the player to move faster. In other words, the speed is **dependent** on the frame rate. To fix this, you need to make sure the speed is **independent** of the frame rate. To do this, all you have to do is multiply your speed by Delta Seconds.

For example, imagine a character with a maximum speed of 100. If one second passes since the last frame, the character will move the full 100 units. If half a second passes, it will move 50 units.

![width=80%](images/t05.png)

This is a frame rate independent implementation. If it was dependent, the character would move 100 units every frame, regardless of the time between frames.

First, let's make the moving up code frame rate independent. Since you'll need to multiply the speed by Delta Seconds, you'll need another input on the **float \* float** node. To do this, click **Add pin**.

Afterwards, create a **Get World Delta Seconds** node and connect it to the input you just added. Repeat the process for the moving right code.

![width=80%](images/i11.png)

Doing this will actually make movement very slow because now you are only using a fraction of the speed each frame. To fix this, all you need to do is increase the speed value. Go ahead and change **Speed** to **2000**. Since movement is now frame rate independent, this value means the player will move at a rate of 2000 units per second.

**Compile** and then click **Play** to test the game. You won't actually notice a difference but you can now rest assured that players across different machines will move at the same rate.

## Key points

 - Avoid **hard coding** where possible as it makes it difficult to modify code later on. This is especially true if you have a lot of classes.
 - **Mappings** allow actors to only care about _when_ an event occurs rather than _how_ it occurs. This also alleviates input hard coding.
 - **Action** events execute when an assigned key is **pressed** or **released**. They are useful for single input actions such as jumping or opening a menu.
 - **Axis** events execute **continuously**, making them useful for actions where you need to hold a key such as moving or shooting.
 - Every input will produce a numerical value called an **axis value**. Its value is determined by the type of input and how you use it. For example, keyboard keys output 1 when pressed and 0 when not pressed. Thumbsticks output a value between -1 and 1 depending on the direction and how far you push it.
 - Unreal will multiply a key's value by it's **scale** before outputting it to the mapping's event. This is useful for inverting a key's value.
 - **Variables** allow you to store values, which you can then use throughout your Blueprint. This is another way to avoid hard coding.
 - **Frame rate independence** is when your logic functions the same across different frame rates.

## Where to go from here?

You should note that the mapping method in this chapter (and book) is not the most flexible way to set up inputs. Though it removed the hard coding from Blueprints, it is still a form of hard coding because it does not support the user changing mappings in-game.

To support runtime mapping, you would need to create a Blueprint to handle adding and removing mappings at runtime. You also need to make sure you save and load those mappings as well. You would also probably need some sort of menu to make it easier for the player to remap keys. If you'd like to learn more about remapping keys at runtime, I'd recommend searching for "**ue4 runtime key binding**."

In the next chapter, you'll create a bullet class and implement shooting functionality into the player Blueprint.