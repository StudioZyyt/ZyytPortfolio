---
title: Fundamentals
draft: false
tags:
  - example-tag
---
 
# Scenes

Everything that runs in your game exists in a scene. When you package your game for a platform, the resulting game is a collection of one or more scenes, plus any platform-­dependent code you add. You can have as many scenes as you want in a project. A scene can be thought of as a level in a game, though you can have multiple levels in one scene file by just moving the player/camera to different points in the scene. It’s also possible to have multipke Scenes loaded simultaniously, which can be useful f.e. when your UI is completely seperated from your Levels in another Scene.

# GameObjects

Virtually everything in your scene is a GameObject. Think of System.Object in the .NET Framework. Almost all types derive from it. The same concept goes for GameObject. It’s the base class for all objects in your Unity scene.

Every GameObject has a Transform component.

In game development, it’s quite common to use vectors, which I’ll cover a bit more in future articles. For now, it’s sufficient to know that Transform.Position and Transform.Scale are both Vector3 objects. A Vector3 is simply a three-dimensional vector; in other words, it’s nothing more than three points—just X, Y and Z. Through these three simple values, you can set an object’s location and even move an object in the direction of a vector.

# Components

You add functionality to GameObjects by adding Components. Everything you add is a Component and they all show up in the Inspector window. There are MeshRender and SpriteRender Components; Components for audio and camera functionality; physics-related Components (colliders and rigidbodies), particle systems, path-finding systems, third-party custom Components, and more. You use a script Component to assign code to an object. Components are what bring your GameObjects to life by adding functionality, akin to the decorator pattern in software development, only much cooler. Another word for Component is MonoBehaviour, which is the base class for most components.

# Writing Code

[https://docs.unity3d.com/ScriptReference/MonoBehaviour.html](https://docs.unity3d.com/ScriptReference/MonoBehaviour.html)

In the prior code example, there are two methods, Start and Update, and the class EnemyHealth inherits from the MonoBehavior base class, which lets you simply assign that class to a GameObject. There’s a lot of functionality in that base class you’ll use, and typically a few methods and properties. The main methods are those Unity will call if they exist in your class.

[https://docs.unity3d.com/Manual/ScriptingImportantClasses.html](https://docs.unity3d.com/Manual/ScriptingImportantClasses.html)