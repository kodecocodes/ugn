```metadata
number: "2"
title: "Chapter 2: Materials"
```

# Chapter 2: Materials

In the last chapter, you added the player’s mesh to the level, but it didn’t look very interesting as it was just a gray color. In this chapter, you’ll add some visual flair to the game by adding color to the player mesh. You’ll also add a background as well. To accomplish both of these tasks, you’ll use **materials**.

Simply put, a material defines the appearance of something. Want an object to be orange and have a glossy sheen? Use a material. How about a multi-colored object where some parts are a shiny metal and the others are a rough fabric? That’s right — use a material. With materials, you can change almost anything relating to an object’s appearance.

To start, you’ll create a material for the player mesh.

>**Note**: This chapter’s project continues from the previous chapter, Chapter 1, “Getting Started”. If you didn’t follow along, or you want to start fresh, please use the starter project from this chapter.
>
>One thing to note is that projects from now on will have auto exposure disabled. Auto exposure adjusts brightness to recreate the effect of human eyes adapting to different brightnesses. This is desirable in some cases, but for a 2D game like this, it doesn’t look great.
>
>If you’re working from your own project, you can disable auto exposure by searching for it in the Project Settings.

## Creating materials

First, navigate to the **Materials** folder. To create a new material, click **Add New** and select **Material**. Name your new material **M_Ship**.

>**Note**: You might be wondering why the material isn’t named something like M_Player instead. One of the great things about materials is that you can apply them to different meshes. For this game, all of the ship meshes will share the same texture, so it makes sense for them to share the same material too.

Next, **double-click** on **M_Ship** to open it in the Material editor.

### Navigating the Material editor

The Material editor contains four main panels:

![width=80%](images/01.png)

1. **Viewport**: Displays a preview of the material. You can zoom by using the **scroll wheel** and rotate by holding **left-click** and moving your mouse.
2. **Details**: All of the properties of the material are in this panel. If you have a node (more on nodes shortly) selected, this panel displays the properties of the selected node.
3. **Graph**: This is where you create and connect nodes. You’ll spend the majority of your time here when working with materials.
4. **Palette**: Displays a list of the nodes you can create. You can also view this list by **right-clicking** an empty spot in the graph.

Next, you’ll look at the building blocks of materials: **Nodes**.

### Material nodes

A node is an object that either **defines** a value or **performs** a function. By connecting nodes together, you can create an **expression**. Here’s a simple example:

![width=80%](images/02.png)

The two nodes on the left define the values **0** and **1**. The node on the right takes two inputs (0 and 1 in this case) and adds them together. You can then use the result elsewhere by using the pin on the right-hand side. For example, you could connect it to a Multiply node.

![width=80%](images/03.png)

This is just a visual way of writing the expression `(0 + 1) * 5`. You can express a large variety of mathematical expressions as node expressions. The example above is elementary, but you can use materials to perform complicated maths too. For example, a popular exercise for learning advanced materials is to simulate an ocean using Gerstner Waves. [TODO: please add an explanation of Gerstner Waves ]

Most nodes will have at least one input pin on the left-hand side and at least one output on the right-hand side. An exception to this is the **Result** node.

### The Result node

The Result node is the tall node you see in your graph when you create a material. All of your nodes will eventually connect to this node. Unreal uses all of these connections to calculate the material’s final appearance. This means that any unconnected nodes **won’t** affect the material.

The number of pins may seem overwhelming, but you’ll only have to use a few of them in this book.

Unreal primarily uses **physically based rendering** (PBR) for its shading. With PBR, you can accurately model how light interacts with a surface making it easier to obtain realistic and consistent visuals. 

Here’s a look at the most important parameters in PBR:

1. **Base Color**: The color of the material. The main ways to define the color are by specifying a color with a node or by using a texture.
2. **Metallic**: Determines how metal-like a surface is. Higher metallic values increase the material’s reflectivity.
3. **Roughness**: Increasing this value will decrease the sharpness of highlights and reflections. Examples of materials with high roughness values are wood and rusted metal.

![width=80%](images/04.png)

> **Note**: The **Specular** input controls the intensity of highlights from things like lights. Artists usually prefer to diffuse the highlights using the Roughness input, so this parameter was omitted from the list above. There are some cases where you’ll want to use the Specular input, but those are rare.

There are other inputs such as Normal and Ambient Occlusion, but the bulk of PBR is in the parameters above. If you’d like to learn more about the other inputs, check out the Unreal Engine documentation.

[TODO: I think you should consider adding these other items to the list rather than splitting it out the way you did. In other words, one list with a small explanation as to which ones are most important and why.]

There’s another input that’s worth mentioning, but it’s not PBR-related. Grouped at the top with the other inputs is **Emissive Color**. The primary way to define color is through the Base Color input, but this is affected by the way the material interacts with light. Emissive Color allows you to specify a color that’s **not** affected by light. This is useful for things such as glowing objects or games with no lighting.

![width=80%](images/05.png)

Now that you know how materials work, it’s time to look at how you can use a material to display a texture.

### Texture mapping

The process of applying a 2D texture to a 3D object is called **texture mapping**. Imagine taking a cube and **unfolding** it into a 2D net. In the world of 3D graphics, this net is called a **UV map** and it’s stored within the mesh’s file. 

Here’s a cube and its UV map:

![width=80%](images/06.png)

> **Note**: Normally, you’d create a UV map in a 3D program such as Maya or Blender. For this book, the meshes already have a UV map, so you don’t need to worry about creating them yourself.

Now, take an image (the texture) and draw the UV map over it. If you cut out the UV map and fold it back into its original shape, you now have a textured mesh.

![width=80%](images/07.png)

> **Note**: Typically, UV maps look like the one in the example, where each face has its own space on the texture. The meshes for this game use a different technique in which the texture is a color palette rather than details. This allows you to easily color certain parts of the mesh by assigning faces to the appropriate part of the texture.

Now that you understand how texture mapping works, you’ll look at how to apply a texture.

### Applying textures

To use a texture, you need a **TextureSample** node. Search for it in the Palette and then drag it into the graph. Alternatively, **right-click** the graph and search for it in the menu.

![width=80%](images/texturesample.png)

Next, you need to select the appropriate texture. Make sure you have the node selected, then go to the Details panel and set **Texture** to **T_Ship**.

![width=80%](images/texturesample2.png)

This game won’t use any lighting, so you’ll need to use Emissive Color rather than Base Color; however, there’s a problem: The TextureSample node has five outputs, so how do you know which one to use? To answer that question, you need to know how colors are typically stored in images.

A popular way to represent a color is to describe it using its red, green and blue components. In other words, you can create any color by adding different amounts of red (**R**), green (**G**) and blue (**B**). Most images store these three values for every texel. If the image also has transparency or an **alpha** channel, the image stores **RGBA** values instead.

>**Note**: A texel is a single unit in an image. For example, when you describe the dimensions of an image, you’re describing it in terms of texels. Most image software uses the term pixel instead, but it’s helpful to make a distinction between the two terms. Therefore, this book used the term pixel to refer to a unit on your screen and texel for a unit on a texture.

The bottom four pins are the individual channels whereas the top pin is the color represented by combining the RGB channels. Since you want to use the actual color, not an individual channel, you need to connect the top pin.

**Drag-click** the **top pin** over to the **Emissive Color** input. Once you see the check icon, release to create a connection.

![width=80%](images/15.png)

There’s one more thing to do before you finish with this material. Even though you’re not using the other pins, Unreal will still calculate lighting for this material. Although the performance cost is usually insignificant, it’s still good practice to cut lighting calculations if you don’t need them. To do so, deselect any nodes to switch the Details panel back to the material settings. Afterward, set **Shading Model** to **Unlit**.

![width=80%](images/16.png)

This disables most of the inputs on the Result node, indicating that the material does not use them.

Finally, go to the Toolbar and click **Apply**. When you click this, Unreal compiles the shaders necessary to run the material. A shader is a small program the engine uses to render a pixel. 

![width=80%](images/17.png)

The material is complete for now, so you can close it. The next step is to apply the material to all of the meshes.

### Applying materials

There are two main ways to apply a material. The first is to apply it globally — that means any instance of that mesh will use the applied material as its default material. The second is to apply it per-instance — that means only the selected instance will use the material. You’ll use the first method for the meshes and the second method for the background.

Up first is the player mesh. Navigate to **Meshes** and open **SM_Player**. This opens the Static Mesh editor. The interface is self-explanatory, so there’s no need to explain it.

Next, go to the Details panel and locate the **Material Slots** section. This section displays a list of material slots for the current mesh. Material slots allow you to assign multiple materials to a mesh. For simplicity, all meshes in this book will have only one material slot.

To apply your material, change the **Element 0** material to **M_Ship**.

![width=80%](images/17-2.png)

Now the ship has some color!

![width=80%](images/18.png)

Close the mesh editor and then repeat the same process to apply your material to the other meshes.

What kind of shoot ’em up would this be if there wasn’t a cool environment for the player to fly through? The next task on the list is to add a background.

## Adding the background

For this game, the background is an image on a plane mesh which means you’ll need another material. Instead of creating a blank material, you can save time by duplicating the ship material.

Navigate to **Materials** and then duplicate **M_Ship** by **right-clicking** it and selecting **Duplicate**. Rename the new material to **M_Background** and then open it.

First, you’ll need to change the texture. Select **TextureSample** and change the texture to **T_Background**.

![width=80%](images/19.png)

Now, you need to apply this material to a plane mesh. Click **Apply** and then go back to the main editor. 

To create a plane mesh, go to the Modes panel and make sure you’re in the **Basic** tab. Now, drag a **Plane** mesh into your Viewport. Rename it to **BackgroundPlane** by selecting it and then pressing **F2**.

To prevent the background from overlapping with the player mesh, you can move it down. Set its **Location** to **(0, 0, -500)**. The plane is also a bit too small, so set its **Scale** to **(40, 40, 1)** to enlarge it.

![width=80%](images/20.png)

Next, you need to apply the background material. Go down to the **Materials** section and set the material to **M_Background**.

![width=80%](images/21.png)

Congratulations, you’ve just created the environment for the entire game!

![width=80%](images/22.png)

Hang on! Don’t celebrate yet. If you look closer, you’ll see the background is squished horizontally. This happens because you’re trying to fit a rectangular image into a square area. You can fix this by widening the plane so that it matches the background’s width. However, since this is a chapter on materials, you’ll fix it using materials!

### Stretching the background image

To fix the squishing, you’ll need to learn a bit more about UV maps.

Unreal evaluates the material graph for _every_ pixel. A common mistake beginners make is thinking that the output of a TextureSample is the texture itself. This is not the case!  When you use a TextureSample, you’re retrieving a single color for the _current_ pixel. Naturally, this also means there must be some parameter that determines which texel to sample. This is where **UV coordinates** come into play.

When you unfold a mesh, you’re essentially taking every vertex and then converting it to a 2D point. These points are called UV coordinates or sometimes just **UVs**. Their values are generally within the 0 to 1 range.

To sample a texel in Unreal, you specify a UV ranging anywhere from (0, 0) to (1, 1). For example, to sample the center texel, you would use (0.5, 0.5).

Since the UV map for a plane encompasses the entire (0, 0) to (1, 1) range, it will sample the entire texture, which is why squishing occurs. To fix this, every pixel needs to decrease its UV coordinate by a factor of four since the background image is four squares wide.

Ready to try it out?

Open **M_Background** and create a **TextureCoordinate** node. This node returns the UV coordinates for the current pixel.

![width=80%](images/23.png)

Next, you’ll need to divide the UV by some constant. Since the texture is only squished horizontally, you need to do this for only the UV’s horizontal component. Create a **Constant2Vector** node and set its value to **(4, 1)**.

![width=80%](images/vector2.png)

> **Note**: You can think of a vector as a collection of numbers. In this case, a Constant2Vector contains two numbers. Similarly, a Constant3Vector and Constant4Vector contain three and four numbers respectively.

Now, create a **Divide** and connect everything like so:

![width=80%](images/24.png)

This increases the texture’s scale by four on the horizontal or U axis. Click **Apply** and then go back to the main editor to check it out.

![width=80%](images/25.png)

The background looks great now, but there’s a lot of artwork going to waste because it doesn’t fit onto the plane. A way to get around this is to scroll the background so the player gets a chance to see all of it. This will also make it seem like the player is flying through the environment. [TODO: You're not really 'getting around this' as much as you are introducing a scrolling background with deliberate intent. It might be better to explain that this is a design/gameplay choice rather than just a reason to show all the art.]

### Scrolling the background

Open **M_Background**, create a **Panner** node, and then modify your graph so it matches this:

![width=80%](images/26.png)

> **Note**: Notice that the TextureCoordinate is gone. This is because the Panner implicitly uses the default UVs, meaning the TextureCoordinate is not required.

Next, you need to specify a speed for the horizontal axis. Select the **Panner** and then go to the Details panel. Once there, set **Speed X** to **1**.

![width=80%](images/27.png)

This will cause the texture to move from right to left. Click **Apply** and then go back to the main editor to check out the scrolling.

![width=80%](images/28.png)

> **Note**: If your background is not scrolling, it might be caused by the Viewport not updating in real-time. To fix this, click **▼** at the top-left of the Viewport and enable **Realtime**.

Great! Now it feels like the ship is flying super fast, but at this speed, the unfortunate pilot would pass out from the amount of gravitational force. You’ll need to slow it down a bit if you want to keep the pilot awake.

You can slow things down by changing the speed value and recompiling. However, as you’ve probably already noticed, compiling takes some time. Simple materials compile quickly but as your material grows, so do the compile times.

Luckily, Unreal provides a way to change values without recompiling. Say hello to your new best friend, the **material instance**.

## Using material instances

You can think of a material instance as a copy of a parent material. The difference is that you cannot edit the material graph in a material instance, but you _can_ change the values of any parameters you create in the parent material — and as mentioned before, you can do this without recompiling!

There are many parameter types, but the ones you’ll use the most are **scalars**, **vectors** and **textures**. For this material, you’ll use scalar parameters, which hold a single numeric value.

Open **M_Background** and create two **ScalarParameter** nodes. Name them **SpeedX** and **SpeedY**.

![width=80%](images/29.png)

You need two parameters because the Panner uses separate speeds for each axis. The problem now is that you have two values, but there’s only one input for speed. Don’t worry, the Panner expects you to provide both speeds as a two-channel vector. For that, you can combine the two parameters using an **AppendVector** node and then connect everything like so:

![width=80%](images/30.png)

Now that you have the parameters, it’s time to create the material instance.

Click **Apply** and then close the material. Now, go to the Content Browser, **right-click** on **M_Background** and select **Create Material Instance**. Rename the instance to **MI_Background** and then open it.

In the Details panel, you’ll see the parameters you just created. To change the scroll speed, enable the **SpeedX** parameter and then change its value to **0.1**.

![width=80%](images/31.png)

Finally, you need to apply the material instance to the plane. Close the material instance and then select the **BackgroundPlane**. Now, set its material to **MI_Background**. The background will now scroll at a safe speed for the pilot.

## Key points

In this chapter, you learned how to:

 - Create a material that displays a texture.
 - Apply a material to all instances and per instance.
 - Scale and pan a texture by manipulating UV coordinates.
 - Create and use material instances to bypass the shader compilation stage.

You’ve only scratched the surface of materials. There are a ton of nodes to explore and even more ways to combine them. As you progress through this book, you’ll work with a handful of the most common ones.

In the next chapter, you’ll start using Blueprints to create the player character.