```metadata
number: "2"
title: "Chapter 2: Materials"
```

# Chapter 2: Materials

In the last chapter, you added the player's mesh to the level but it didn't look very interesting as it was just a gray color. In this chapter, you will add some visual flair to the game by adding color to the player mesh and adding a background as well. To accomplish both of these tasks, you will need to use **materials**.

What is a material, you ask? Simply put, a material defines the appearance of something. Want an object to be orange and have a glossy sheen? Use a material. How about a multi-colored object where some parts are a shiny metal and the others are a rough fabric? Use a material! With materials, you can change almost anything relating to an object's appearance.

To start, let's create a material for the player mesh.

>**Note**: This chapter’s project continues from the previous chapter, Chapter 1, "Getting Started." If you didn’t follow along, or you want to start fresh, please use the starter project from this chapter.
>
>One thing to note is that projects from now on will have auto exposure disabled. Auto exposure adjusts brightness to recreate the effect of human eyes adapting to different brightnesses. This is desirable in some cases but for a 2D game like this, it doesn't look great.
>
>If you're working from your own project, you can disable auto exposure by searching for it in the Project Settings.

## Creating materials

First, navigate to the **Materials** folder. To create a new material, click **Add New** and select **Material**. Name your new material to **M_Ship**.

>**Note**: You might be wondering why the material isn't named something like M_Player instead. Well, one of the great things about materials is that you can apply them to different meshes. For this game, all the ship meshes will share the same texture so it makes sense for them to share the same material too.

Next, **double-click** on **M_Ship** to open it in the material editor.

### Navigating the material editor

The material editor contains four main panels:

![unreal engine wounds](images/01.png)

1. **Viewport**: Displays a preview of the material. You can zoom by using the **scroll wheel** and rotate by holding **left-click** and moving your mouse.
2. **Details**: All the properties of the material are in this panel. If you have a node (more on nodes shortly) selected, this panel will display the properties (if any) of the selected node.
3. **Graph**: This is where you create and connect nodes. You will spend the majority of your time here when working with materials.
4. **Palette**: Displays a list of all the nodes you can create. You can also view this list by **right-clicking** an empty spot in the graph.

Next, let's take a look at the building blocks of materials: **Nodes**.

### Material nodes

A node is an object that either **defines** a value or **performs** a function. By connecting nodes together, you create an **expression**. Here's a simple example:

![unreal engine wounds](images/02.png)

The two nodes on the left define the values **0** and **1**. The node on the right takes two inputs (0 and 1 in this case) and adds them together. You can then use the result elsewhere by using the pin on the right-hand side. For example, you could connect it to a Multiply node.

![unreal engine wounds](images/03.png)

As you can see, this is actually just a visual way of writing the expression `(0 + 1) * 5`. In fact, you can express a large variety of mathematical expressions as node expressions. The example above is very simple but you can actually use materials to perform complex maths too. For example, a popular exercise for learning advanced materials is to simulate an ocean using Gerstner Waves.

Most nodes will have at least one input pin on the left-hand side and at least one output on the right-hand side. An exception to this is the **Result** node.

### The Result node

The Result node is the tall node you see in your graph when you create a material. This is where all your nodes will eventually connect to. Unreal will use all these connections to calculate the material's final appearance. This means any unconnected nodes will **not** have an effect on the material.

The amount of pins may seem overwhelming but luckily, you'll only have to use a few of them in this book.

Unreal primarily uses **physically based rendering** (PBR) for its shading. This is a way to accurately model how light interacts with a surface, meaning it is much easier to obtain realistic and consistent visuals. Let's look at the most important parameters in PBR:

1. **Base Color**: The color of the material. The main ways to define the color are by specifying a color with a node or by using a texture.
2. **Metallic**: How “metal-like” a surface is. Higher metallic values increase the material's reflectivity.
3. **Roughness**: Increasing this will decrease the sharpness of highlights and reflections. Examples of materials with high roughness would be wood and rusted metal.

>**Note**: The **Specular** input controls the intensity of highlights from things such as lights. The reason I've left it out of the list is because artists usually prefer to just diffuse the highlights using the Roughness input. There are some cases where you'll want to use the Specular input but those are rare.

![unreal engine wounds](images/04.png)

Of course there are other inputs such as Normal and Ambient Occlusion but the bulk of PBR is in the parameters above. I won't get into what the other inputs do in this book but if you'd like to learn more, check out the Unreal Engine documentation.

There is also one more input I'd like to mention that is not PBR-related. Grouped together at the top with the other inputs is the **Emissive Color** input. The primary way to define color is through the Base Color input but this is affected by the way the material interacts with light. Emissive Color allows you to specify a color that is **not** affected by light. This is useful for things such as glowing objects or games with no lighting.

![unreal engine wounds](images/05.png)

Now that you know how materials work, let's look at how you can use a material to display a texture. The process of applying a 2D texture to a 3D object is called **texture mapping**.

### Texture mapping

Let's take a trip down memory lane to our geometry class in elementary school. Specifically, I want you to remember the exercise of taking a cube and **unfolding** it into a 2D "net". In the world of 3D graphics, this net is called a **UV map** and it is stored within the mesh's file. Here is a cube and its UV map:

![unreal engine wounds](images/06.png)

>**Note**: Normally, you would create a UV map in a 3D program such as Maya or Blender. For this book, all the meshes in this book already have a UV map so you don't have to worry about creating them yourself.

Now, take an image (the texture) and draw the UV map over it. If you cut out the UV map and fold it back into its original shape, you now have a textured mesh!

![unreal engine wounds](images/07.png)

>**Note**: Typically, UV maps usually look like the above where each face has its own space on the texture. The meshes for this game use a different technique in which the texture is a color palette rather than details. This allows you to easily color certain parts of the mesh simply by assigning faces to the appropriate part of the texture.

This is how texture mapping works. Next, let's look at how to apply a texture.

### Applying textures

To use a texture, you will need a **TextureSample** node. To create one, search for it in the Palette and then drag it into the graph. Alternatively, **right-click** the graph and search for it in the menu.

![unreal engine wounds](images/texturesample.png)

Next, you need to select the appropriate texture. Make sure you have the node selected and then go to the Details panel. Afterwards, set **Texture** to **T_Ship**.

![unreal engine wounds](images/texturesample2.png)

This game won't use any lighting so you'll need to use Emissive Color rather than Base Color. However, there is a problem! The TextureSample node has five outputs so which one do you use? To answer that, you need to know how colors are typically stored in images.

A popular way to represent a color is to describe it in terms of its red, green and blue components. In other words, you can create any color by adding different amounts of red (**R**), green (**G**) and blue (**B**) together. Most images will store these three values for every texel. If the image also has transparency or an **alpha** channel, the image will store **RGBA** values instead.

>**Note**: A texel is a single unit in an image. For example, when you describe the dimensions of an image, you are describing it in terms of texels. Most image software will use the term "pixel" instead but I think it's helpful to make a distinction between the two terms. For this book, I'll use the term "pixel" to refer to a unit on your screen and "texel" for a unit on a texture.

As you've probably guessed, the bottom four pins are the individual channels while the top pin is the color represented by combining the RGB channels. Since you want to use the actual color, not an individual channel, you'll want to connect the top pin.

To do this, **drag-click** the **top pin** over to the **Emissive Color** input. Once you see the check icon, release to create a connection.

![unreal engine wounds](images/15.png)

There's one more thing to do before you finish with this material. Even though you are not using the other pins, Unreal will still calculate lighting for this material. The performance cost of this is usually insignificant but it's still good practice to cut lighting calculations if you don't need them. To do this, deselect any nodes to switch the Details panel back to the material settings. Afterwards, set **Shading Model** to **Unlit**.

![unreal engine wounds](images/16.png)

This will disable most of the inputs on the Result node, indicating that the material does not use them.

Finally, go to the Toolbar and click **Apply**. When you click this, Unreal will compile the shaders necessary to run the material. A shader is basically a small program the engine uses to render a pixel. 

![unreal engine wounds](images/17.png)

The material is complete for now so go ahead and close it. The next step is to apply the material to all of the meshes.

### Applying materials

There are two main ways of applying a material. The first is to apply it globally which means any instance of that mesh will use the applied material as its default material. The second is to apply it per-instance which means only the selected instance will use the material. We'll do the first method for the meshes and the second method for the background.

Up first is the player mesh. Navigate to the **Meshes** folder and open **SM_Player**. This will open the static mesh editor. The interface is pretty self-explanatory so I'll skip over it.

Next, go to the Details panel and locate the **Material Slots** section. This section will display a list of material slots for the current mesh. Material slots allow you to assign multiple materials to a mesh. For simplicity, all meshes in this book will only have one material slot.

To apply your material, change the **Element 0** material to **M_Ship**.

![unreal engine wounds](images/17-2.png)

Now the ship has some color!

![unreal engine wounds](images/18.png)

Close the mesh editor and then repeat the same process to apply your material to the other meshes.

What kind of shoot 'em up would this be if there wasn't a cool environment for the player to fly through? The next task on the list is to add a background.

## Adding the background

For this game, the background will be an image on a plane mesh which means you'll need another material. Instead of creating a blank material, you can save time by duplicating the ship material. Navigate to the **Materials** folder and then duplicate **M_Ship** by **right-clicking** it and selecting **Duplicate**. Rename the new material to **M_Background** and then open it.

First, you will need to change the texture. Select the **TextureSample** and change the texture to **T_Background**.

![unreal engine wounds](images/19.png)

Now let's apply this material to a plane mesh. Click **Apply** and then go back to the main editor. To create a plane mesh, go to the Modes panel and make sure you are in the **Basic** tab. Afterwards, drag a **Plane** mesh into your Viewport. Rename it to **BackgroundPlane** by selecting it and then pressing **F2**.

To prevent the background from overlapping with the player mesh, you can simply move it down. To do this, set its **Location** to **(0, 0, -500)**. The plane is also a bit too small so set its **Scale** to **(40, 40, 1)** to enlarge it.

![unreal engine wounds](images/20.png)

Next, you need to apply the background material. Go down to the **Materials** section and set the material to **M_Background**.

![unreal engine wounds](images/21.png)

Congratulations, you've just created the environment for the entire game!

![unreal engine wounds](images/22.png)

Don't celebrate yet though! If you look closer, you'll see the background is actually squished horizontally. This is because you are trying to fit a rectangular image into a square area. You can easily fix this by widening the plane so that it matches the background's width. But since this is a chapter on materials, let's fix it using materials!

### Stretching the background image

To fix the squishing, you'll need to learn a bit more about UV maps.

An important thing to note is that Unreal evaluates the material graph for _every_ pixel. A common mistake beginners make is thinking the output of a TextureSample is the texture itself. This is not the case!  When you use a TextureSample, you are actually retrieving a single color for the _current_ pixel. Naturally, this also means there must be some parameter that determines which texel to sample. This is where **UV coordinates** come in.

When you unfold a mesh, you are essentially taking every vertex and then converting it to a 2D point. These points are called UV coordinates or sometimes just **UVs**. Their values are generally within the 0 to 1 range.

To sample a texel in Unreal, you specify a UV ranging anywhere from (0, 0) to (1, 1). For example, to sample the center texel, you would use (0.5, 0.5).

Since the UV map for a plane encompasses the entire (0, 0) to (1, 1) range, it will sample the entire texture, which is why squishing occurs. To fix this, every pixel needs to decrease its UV coordinate by a factor of four since the background image is four squares wide.

Let's try it out. Open **M_Background** and create a **TextureCoordinate** node. This node will return the UV coordinates for the current pixel.

![unreal engine wounds](images/23.png)

Next, you'll need to divide the UV by some constant. Since the texture is only squished horizontally, you only need to do this for the UV's horizontal component. Create a **Constant2Vector** node and set its value to **(4, 1)**.

![unreal engine wounds](images/vector2.png)

>**Note**: You can think of a vector as a collection of numbers. In this case, a Constant2Vector contains two numbers. Similarly, a Constant3Vector and Constant4Vector contain three and four numbers respectively.

Afterwards, create a **Divide** and connect everything like so:

![unreal engine wounds](images/24.png)

This will increase the texture's scale by four on the horizontal or U axis. Click **Apply** and then go back to the main editor to check it out.

![unreal engine wounds](images/25.png)

The background looks great now but there's a lot of artwork going to waste because it doesn't fit onto the plane. A way to get around this is to scroll the background so the player gets a chance to see all of it. This will also make it seem like the player is flying through the environment.

### Scrolling the background

Open **M_Background** and create a **Panner** node. Afterwards, modify your graph to this:

![unreal engine wounds](images/26.png)

>**Note**: Notice that the TextureCoordinate is gone. This is because the Panner implicitly uses the default UVs, meaning the TextureCoordinate is not required.

Next, you need to specify a speed for the horizontal axis. Select the **Panner** and then go to the Details panel. Afterwards, set **Speed X** to **1**.

![unreal engine wounds](images/27.png)

This will cause the texture to move from right to left. Click **Apply** and then go back to the main editor to check out the scrolling.

![unreal engine wounds](images/28.png)

>**Note**: If your background is not scrolling, it might be caused by the Viewport not updating in realtime. To fix this, click the **▼** at the top-left of the Viewport and enable **Realtime**.

Now it feels like the ship is flying really fast! Unfortunately, at this speed, the poor pilot will probably pass out from the amount of gravitational force so let's slow it down. Obviously, you can do this by changing the speed value and recompiling. But as you've probably already noticed, compiling takes some time. Simple materials compile quickly but as your material grows, so do the compile times.

Luckily, Unreal provides a way to change values without recompiling. Say hello to your new best friend, the **material instance**.

## Using material instances

You can think of a material instance as a copy of a parent material. The difference is that you cannot edit the material graph in a material instance but you _can_ change the values of any parameters you create in the parent material. And as mentioned before, you can do this without recompiling.

There are a lot of parameter types but the ones you will use the most are **scalars**, **vectors** and **textures**. For this material, you'll need to use scalar parameters. These hold a single numeric value.

Open **M_Background** and then create two **ScalarParameter** nodes. Rename them to **SpeedX** and **SpeedY**.

![unreal engine wounds](images/29.png)

The reason you need two parameters is because the Panner uses separate speeds for each axis. The problem now is that you have two values but there is only one input for speed. This is because the Panner expects you to provide both speeds as a two channel vector. To do this, combine the two parameters using an **AppendVector** node and then connect everything like so:

![unreal engine wounds](images/30.png)

Now that you have the parameters, it's time to create the material instance. Click **Apply** and then close the material. Afterwards, go to the Content Browser, **right-click** on **M_Background** and select **Create Material Instance**. Rename the instance to **MI_Background** and then open it.

In the Details panel, you'll see the parameters you just created. To change the scroll speed, enable the **SpeedX** parameter and then change its value. For this game, set it to **0.1**.

![unreal engine wounds](images/31.png)

Finally, you need to apply the material instance to the plane. Close the material instance and then select the **BackgroundPlane**. Afterwards, set its material to **MI_Background**. The background will now be scrolling at a safe speed for the pilot.

## Key points

In this chapter, you learned how to:

 - Create a material that displays a texture.
 - Apply a material to all instances and per instance.
 - Scale and pan a texture by manipulating UV coordinates.
 - Create and use material instances to bypass the shader compilation stage.

So far, you have only scratched the surface of materials. As you already saw, there are a ton of nodes to explore and even more ways to combine them. Obviously, there are way too many to cover in this book but you will learn a handful of the most common ones later on.

In the next chapter, you will start using Blueprints to create the player character.