```metadata
number: "1"
title: "Chapter 1: Getting Started"
```

# Chapter 1: Getting Started

[TODO: You need an introduction here explaining what the chapter is about.]

## Installing Unreal Engine 4

To install the Unreal Engine, you first need to install the Epic Games Launcher. Head over to [https://www.unrealengine.com/download/](https://www.unrealengine.com/download/) and download the appropriate installer for your operating system.

![width=80%](images/03.png)

After installing and letting the launcher update, you’ll see the Sign In window.

![width=80%](images/04.png)

If you don’t already have an account, click the **Sign Up** link at the bottom to create a free one.

> **Note**: You can also use the launcher without an account by clicking **Sign In Later**, but you won’t be able to access any items you purchase from the marketplace. The marketplace is an online store where users can buy content for their projects including models, textures, code and more.

After creating an account and signing in, go to the **Unreal Engine** tab and click **Install Engine**.

![width=80%](images/05.png)

This prompts you to select an install location for the engine. You can also choose to include or exclude certain components. To view these components, click the **Options** button.

![width=80%](images/06.png)

> **Note**: Epic Games is constantly updating Unreal Engine, so your engine version may be different. For this book, you’ll need at least version 4.21.

The default option selections are **Starter Content**, **Templates and Feature Packs** and **Engine Source**.

![width=80%](images/07.png)

Here’s a look at each one:

 - **Starter Content**: This is a free collection of assets that you can use in your projects. It includes content like models and textures. You can use these as placeholder assets or include them in your released project.
 - **Templates and Feature Packs**: A collection of templates to help get you started with a specific type of project. Common templates are the third and first-person templates.
 - **Engine Source**: This component allows you to browse the source code for the engine. This is useful if you’re familiar with C++ and want to debug or see how the engine is programmed.

Scrolling down the list, you’ll see multiple platform components. These components allow you to package your project for the specified platform. If you don’t plan to develop for a specific platform, you can exclude it.

![width=80%](images/08.png)

To follow along with this book, the only required component is **Core Components**. After making your component selections, click **Apply** and then **Install**. Once the installation is complete, the next step is to create your first project.

## Creating a project

Click the **Launch** button to open the Project Browser. After the Project Browser opens, verify you’re in the **New Project** tab. You can now close the Epic Games Launcher.

![width=80%](images/09.png)

The projects in this book are made entirely in Blueprints, so make sure you’re in the **Blueprints** tab. 

[TODO: What is Blueprints? It might be a good idea to add a short explanation here.]

Here, you can select the template you want to use as your starting point. You’ll be creating your first project from scratch, so select the **Blank** template.

In the middle of this window, you’ll see additional settings.

![width=80%](images/10.png)

The first two options disable some rendering features. This is useful if you’re targeting lower-end hardware such as mobile devices. The third option lets you choose whether or not to include the starter content. Leave these options at their default setting.

Finally, there’s a section to specify the project folder’s location and project name. Choose a location for your project and set the project name to **SpaceShooter**. This name does not represent the game’s title, so don’t worry if you want to change the title later. After setting the name, click **Create Project**.

Unreal creates the project and then opens the main editor, also known as the level editor. You’ll be spending a lot of time here, so it’s best to get acquainted with it.

## Navigating the interface

The main editor contains six main panels:

![width=80%](images/11.png)

1. **Toolbar**: Contains a variety of commonly accessed functions. In this book, you’ll mostly use the Play function.
2. **Modes**: Here, you can select between tools such as the Landscape tool and the Foliage tool. The default tool is the Place tool which allows you to place different types of objects, such as lights and cameras, into your levels.
3. **Viewport**: This is the view of your level. You can look around by holding **right-click** and moving your mouse. To move the camera, hold right-click and use the **WASD** keys.
4. **World Outliner**: Displays a complete list of objects that are in the current level.
5. **Details**: When you select an object, its properties are displayed here.
6. **Content Browser**: This is where you import, create and manage your project files.

Because this project will have lots of files, you need to create some folders to help with organization, which is what you’ll do next.

## Organizing your project

It’s best to create a folder named after your project and put any subsequent folders inside of that. This allows your project to remain separate from any templates or marketplace items you may want to add later. 

> **Note**: Epic requires marketplace items to have a single top-level folder as well.

To create a folder, go to the Content Browser and click the green **Add New** button. Select **New Folder** from the menu and name the folder **SpaceShooter**.

For this game, you’ll keep the folder structure simple and separate assets by their type.

 **Double-click** the **SpaceShooter** folder to open it, and then create the following folders inside: **Audio**, **Blueprints**, **Maps**, **Materials**, **Meshes**, **ParticleSystems**, **Textures** and **UI**.

To display a tree view of your folders, click the highlighted button in the Content Browser.

![width=80%](images/12.png)

When you’re done, your folder structure will look like this:

![width=80%](images/13.png)

Now that you have your folders set up, it’s time to start importing some assets.

## Importing assets

First, you’ll need to locate the assets to import. You can find them by navigating to the folder for this chapter and opening the **resources** folder. 

Inside **resources**, you’ll see the following files:

 - **SM_Boss.fbx**
 - **SM_EnemyA.fbx**
 - **SM_EnemyB.fbx**
 - **SM_Player.fbx**
 - **T_Background.png**
 - **T_Bullet.png**
 - **T_Ship.png**

Unreal mainly uses the FBX file format for anything 3D related such as models and animations. For this project, the FBX files are the models for the player and enemies and are prefixed with **SM_**, which stands for **static mesh**. This is the term Unreal uses for non-animated models. The files in this book all have an appropriate prefix to help with organization.

The PNG files are the textures for the background, bullets and ships. The player and enemies share the same texture.

[TODO: You might want to put that information into a list since you already have the list. ]

> **Note**: Unreal also supports other image formats such as JPG and TGA, but for this book, all images are PNGs.

You’ll start with importing the static meshes. Select all of the FBX files and drag them into the **Meshes** folder within Unreal.

![width=80%](images/14.png)

This brings up a window where you can change import options. For now, you can ignore most of these except for one: Import Materials.

By default, Unreal creates a material (more on materials in the next chapter) for imported meshes. However, in the next chapter, you’ll create your own materials, so uncheck **Import Materials** to prevent Unreal from creating a material.

![width=80%](images/15.png)

Next, click **Import All** to import all of the meshes with the same settings. 

Notice that you now have four files inside of Meshes.

![width=80%](images/16.png)

When you import a file, Unreal won’t save it into your project until you explicitly do so. 

To save files, **right-click** the file and select **Save**. Alternatively, you can save all files at once by selecting **File ▸ Save All**. 

Up next are the textures. Repeat the same process as before, but import the textures into **Textures** instead. Remember to save those as well.

> **Note**: Make sure you save often!

For now, that’s it for the assets. Your next step is to add a mesh to the level.

## Adding meshes to the level

This game won’t need any of the objects currently in the level, so you can start with a blank slate instead. To create a new empty level, select **File ▸ New Level** and then choose **Empty Level**.

![width=80%](images/17.png)

Now, add the player model to the level. Navigate to the **Meshes** folder and drag **SM_Player** into the Viewport.

![width=80%](images/18.png)

Easy peasy!

Now that you have an object in the level, you can **translate** (move), **rotate** and **scale** (resize) it. To do this, make sure you have the object selected, and then use the following keys to activate their assigned tools:

 - **W**: Translate
 - **E**: Rotate
 - **R**: Scale

![width=80%](images/19.png)

From here, you can **drag** the handles to use the activated tool. For example, if you have the Translate tool activated, you can move an object along the X-axis by dragging the red arrow. If you want to move on the Y-axis or Z-axis, use the green and blue arrows respectively.

You can also manually type in values for each of these properties. To do this, select the object and then go to the **Transform** section in the Details panel.

For this game, set the **Location** of the player model to **(0, 0, 0)**. This will be the player’s starting location.

![width=80%](images/20.png)

Finally, save the level by selecting **File ▸ Save Current**. This prompts you to specify a save location and level name. Select **Maps** and set the name to **GameLevel**. Afterward, click **Save**.

![width=80%](images/18.5.png)

>**Note**: When you reopen your project, Unreal loads a new level instead of a specific level. If you’d like Unreal to open a specific level instead, go to **Edit ▸ Project Settings**. Select **Maps & Modes** and set the **Editor Startup Map**.
>
>![width=80%](images/21.png)

## Key points

That’s all for this chapter! So far, you learned how to:

 - Install Unreal Engine 4 and create a new project.
 - Navigate the main editor’s interface.
 - Import assets and organize them.
 - Add a mesh to the level and adjust its location, rotation and scale.

Right now, the player and level are looking a little dull. In the next chapter, you’ll learn how to spice up the visuals using **materials**.