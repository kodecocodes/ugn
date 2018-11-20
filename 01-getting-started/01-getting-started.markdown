```metadata
number: "1"
title: "Chapter 1: Getting Started"
```

# Chapter 1: Getting Started

## Installing Unreal Engine 4

To install the engine, you will first need to install the Epic Games Launcher. Head over to https://www.unrealengine.com/download/ and download the installer for your operating system.

![unreal engine wounds](images/03.png)

After installing and letting the launcher update, you will see the sign in window.

![unreal engine wounds](images/04.png)

If you don't already have an account, you can sign up for a free account by clicking the **Sign Up** link at the bottom. You can also use the launcher without an account by clicking **Sign In Later** but you won't be able to access any items you purchase from the marketplace.

>**Note**: The marketplace is an online store where users can buy content for their projects. such as models, textures, code and more.

After signing in, make sure you are in the **Unreal Engine** tab and then click **Install Engine**.

![unreal engine wounds](images/05.png)

This will prompt you to choose an install location for the engine. You can also choose to include or exclude certain components, which you can view by clicking the **Options** button.

![unreal engine wounds](images/06.png)

>**Note**: Epic Games is constantly updating Unreal Engine so your engine version may be different. As long as you have at least version 4.21, you should be set for this book.

The default selections are **Starter Content**, **Templates and Feature Packs** and **Engine Source**.

![unreal engine wounds](images/07.png)

Here's what each of these are for:

 - **Starter Content**: This is a collection of assets that you can use for free in your projects. It includes content such as models and textures. You can use these as placeholder assets or even include them in your released project.
 - **Templates and Feature Packs**: A collection of templates to help get you started with a specific type of project. Common templates to use are the third and first-person templates.
 - **Engine Source**: This component will allow you to browse the source code for the engine. This is useful if you are familiar with C++ and would like to debug or see how the engine is programmed.

Scrolling down the list, you will see multiple platform components. These will allow you to package your project for the specified platform. If you don’t plan on developing for a specific platform, feel free to exclude it.

![unreal engine wounds](images/08.png)

To follow this book, the only required component is **Core Components**. After you have selected your components, click **Apply** and then **Install**. Once you have finished installing, the next step is to create your first project!

## Creating a project

Click the **Launch** button to open the Project Browser. Once the Project Browser opens, make sure you are in the **New Project** tab. Feel free to close the Epic Games Launcher at this point.

![unreal engine wounds](images/09.png)

Since the projects in this book are made entirely in Blueprints, make sure you are in the **Blueprints** tab. Here, you can select a template to start from. For this project, you'll be creating it from scratch so select the **Blank** template.

Further below, you will find additional settings.

![unreal engine wounds](images/10.png)

The first two options will simply disable some rendering features. This is useful if you are targeting lower-end hardware such as mobile devices. The third option will let you choose whether to include the starter content. Leave these options at their default setting.

Finally, there is a section to specify the project folder's location and project name. Choose a location for your project and set the project name to **SpaceShooter**. This name does not represent the game’s title so don’t worry if you want to change the title later on. Once you have set the name, click **Create Project**.

Unreal will create the project and then open the main editor (also known as the level editor). You'll be spending a lot of time here so it's best to get acquainted with it!

## Navigating the interface

The main editor contains six main panels:

![unreal engine wounds](images/11.png)

1. **Toolbar**: Contains a variety of commonly accessed functions. In this book, the one you will use the most is the Play function.
2. **Modes**: Here, you can select between tools such as the Landscape Tool and the Foliage Tool. The default tool is the Place tool which allows you to place many different types of objects into your level such as lights and cameras.
3. **Viewport**: This is the view of your level. You can look around by holding **right-click** and moving your mouse. To move the camera, hold right-click and use the **WASD** keys.
4. **World Outliner**: Displays a list of all the objects in the current level.
5. **Details**: Any object you select will have its properties displayed here.
6. **Content Browser**: This is where you import, create and manage your project files.

Before you dive into anything, let's take a closer look at the Content Browser. This project will have a lot of files later on so let's create a few folders to help with organization.

## Organizing your project

First, I recommend always creating a folder named after your project and then putting any subsequent folders in it. This will allow your project to remain separate from any templates or marketplace items you may want to add later on. In fact, Epic requires marketplace items to have a single top level folder as well.

To create a folder, go to the Content Browser and click the green **Add New** button. Select **New Folder** from the menu and name the folder to **SpaceShooter**.

For this game, we'll keep the folder structure simple and separate assets by their type. **Double-click** the **SpaceShooter** folder to open it and then create the following folders inside: **Audio**, **Blueprints**, **Maps**, **Materials**, **Meshes**, **ParticleSystems**, **Textures** and **UI**.

To display a tree view of your folders, click the highlighted button in the Content Browser.

![unreal engine wounds](images/12.png)

If you've created the folders correctly, your folder structure will look like this:

![unreal engine wounds](images/13.png)

Now that you have your folders set up, it's time to start importing some assets.

## Importing assets

First, you will need to locate the assets to import. You can find them by navigating to the folder for this chapter and opening the **resources** folder. Inside, you will see the following files:

 - **SM_Boss.fbx**
 - **SM_EnemyA.fbx**
 - **SM_EnemyB.fbx**
 - **SM_Player.fbx**
 - **T_Background.png**
 - **T_Bullet.png**
 - **T_Ship.png**

Unreal mainly uses the FBX file format for anything 3D related such as models and animations. In this case, the FBX files are the models for the player and enemys. I have prefixed them with **SM_** which stands for **static mesh**. This is the term Unreal uses for non-animated (hence the word "static") models. All files in this book will have an appropriate prefix to help with organization.

The PNG files are the textures for the background, bullets and ships (the player and enemies share the same texture).

>**Note**: Unreal also supports other image formats such as JPG and TGA but for this book, all images will be PNGs.

Let's start with importing the static meshes. Select all the FBX files and then drag them into the **Meshes** folder within Unreal.

![unreal engine wounds](images/14.png)

This will bring up a window where you can change import options. You can ignore most of these for now but there is one option you should change. By default, Unreal will create a material (more on materials in the next chapter) for imported meshes. However, in the next chapter, you will create your own materials so uncheck **Import Materials** to prevent Unreal from creating a material.

![unreal engine wounds](images/15.png)

Next, click **Import All** to import all the meshes with the same settings. Four files will now appear in your Meshes folder.

![unreal engine wounds](images/16.png)

When you import a file, Unreal does not actually save it into your project until you explicitly do so. You can save files by **right-clicking** the file and selecting **Save**. You can also save all files at once by selecting **File ▸ Save All**. Make sure you save often!

Up next are the textures. Repeat the same process but import them into the **Textures** folder instead. Remember to save those as well!

Those are all the assets you'll need for now. Before we close out this chapter, let's add a mesh to the level.

## Adding meshes to the level

This game won't need any of the objects currently in the level so let's start with a blank slate instead. Create a new empty level by selecting **File ▸ New Level** and then selecting **Empty Level**.

![unreal engine wounds](images/17.png)

Now, let's add the player model to the level. Navigate to the **Meshes** folder and then drag **SM_Player** into the Viewport.

![unreal engine wounds](images/18.png)

Easy peasy! Now that you have an object in the level, you can **translate** (move), **rotate** and **scale** (resize) it. To do this, make sure you have the object selected and then use the following keys to activate their assigned tools:

 - **W**: Translate
 - **E**: Rotate
 - **R**: Scale

![unreal engine wounds](images/19.png)

From here, you can **drag** the handles to use the activated tool. For example, if you have the Translate tool activated, you can move an object along the X-axis by dragging the red arrow. If you want to move on the Y-axis or Z-axis, use the green and blue arrows respectively.

You can also manually type in values for all of these properties. To do this, select the object and then go to the **Transform** section in the Details panel. For this game, set the **Location** of the player model to **(0, 0, 0)**. This will be the player's starting location.

![unreal engine wounds](images/20.png)

Finally, save the level by selecting **File ▸ Save Current**. This will prompt you to give specify a save location and level name. Select the **Maps** folder and set the name to **GameLevel**. Afterwards, click **Save**.

![unreal engine wounds](images/18.5.png)

>**Note**: When you reopen your project, Unreal will load a new level instead of a specific level. If you would like Unreal to open a specific level instead, go to **Edit ▸ Project Settings**. Afterwards, open the **Maps & Modes** page and set the **Editor Startup Map**.
>
>![unreal engine wounds](images/21.png)

## Key points

That's all for this chapter! So far, you have learned how to:

 - Install Unreal Engine 4 and create a new project.
 - Navigate the main editor's interface.
 - Import assets and organize them.
 - Add a mesh to the level and adjust its location, rotation and scale.

Right now, the player and level are looking a little dull. In the next chapter, you'll learn how to spice up the visuals using **materials**.