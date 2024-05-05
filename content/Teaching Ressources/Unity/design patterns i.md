---
title: Design Patterns I
draft: true
tags:
  - unity
  - design-patterns
---

#  [Singleton](https://gameprogrammingpatterns.com/singleton.html)

_Design Patterns_ summarizes Singleton like this:

> Ensure a class has one instance, and provide a global point of access to it.

There are times when a class cannot perform correctly if there is more than one instance of it. Also, having a global point of access can be neccessary for certain systems.

In the short term, the Singleton pattern is relatively benign. Like many design choices, we pay the cost in the long term.

- It’s a global variable.
    - When games were still written by a couple of guys in a garage, pushing the hardware was more important than ivory-tower software engineering principles. Old-school C and assembly coders used globals and statics without any trouble and shipped good games.
    - **They make it harder to reason about code.**
    - **They encourage coupling.**
    - **They aren’t concurrency-friendly.**
- It solves two problems even when you just have one.
- Lazy initialization is not always good, though this is mostly avoidable.

## What can we do?

- **See if you need the class at all.** `MonsterAudioManagerManager`
    
    - While caretaker classes are sometimes useful, often they just reflect unfamiliarity with OOP.
    - How many instances of `BulletManager` do you need? The answer here is _zero,_ actually. After all, OOP is about letting objects take care of themselves.
    
    # Code Example
    
    ```csharp
    class Bullet
    {
        public int X { get; set; }
        public int Y { get; set; }
    }
    
    class BulletManager 
    {
        public Bullet CreateBullet(int x, int y) 
        {
            Bullet newBullet = new Bullet();
            newBullet.X = x;
            newBullet.Y = y;
            return newBullet;
        }
    
        public void move(Bullet bullet)
        {
            bullet.X = bullet.X + 5;
        }
    }
    ```
    
    ```csharp
    class Bullet
    {
        public int X 
    			{ get; set; }
        public int Y 
    			{ get; set; }
    
        public Bullet(int x, int y)
        {
            X = x;
            Y = y;
        }
    
        public void move() 
        {
            X = X + 5;
        }
    }
    ```
    
- **Get it from something already global.**
    
    - The goal of removing _all_ global state is admirable, but rarely practical. Most codebases will still have a couple of globally available objects, such as a single `Game` or `World` object representing the entire game state.
- **Service Locator / Dependency Injection**
    
    - Service Locator pattern tries to improve on the issues that the singleton pattern poses. Conversely, when used poory, it carries with it all of the baggage of the Singleton pattern with worse runtime performance.
    - Dependency Injection is a specific version of the inversion of control pattern, where implementations are passed into an object through contrstructors/setters/etc.
    - Both of these a great if used properly, but require some getting used to and will generate overhead in small projects. Controversial opinion: For small projects or prototypes, using the Singleton Pattern (sparingly!) is totally ok.
    - [https://gameprogrammingpatterns.com/service-locator.html](https://gameprogrammingpatterns.com/service-locator.html)
    - [https://github.com/modesttree/Zenject](https://github.com/modesttree/Zenject)

# [Observer](https://gameprogrammingpatterns.com/observer.html)

You can’t throw a rock at a computer without hitting an application built using the [Model-View-Controller](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) architecture, and underlying that is the Observer pattern. Observer is so pervasive that Java put it in its core library (`[java.util.Observer](<http://docs.oracle.com/javase/7/docs/api/java/util/Observer.html>)`) and C# baked it right into the _language_ (the `[event](<http://msdn.microsoft.com/en-us/library/8627sbea.aspx>)` keyword).

This is tricky to implement cleanly since we have such a wide range of achievements that are unlocked by all sorts of different behaviors. If we aren’t careful, tendrils of our achievement system will twine their way through every dark corner of our codebase. Sure, “Fall off a Bridge” is somehow tied to the physics engine, but do we really want to see a call to `unlockFallOffBridge()` right in the middle of the linear algebra in our collision resolution algorithm?

That’s what the observer pattern is for. It lets one piece of code announce that something interesting happened _without actually caring who receives the notification_.

One very well known type of event in Unity is the `OnClick()` event on certain UI Objects. The reason this event is visible in the Inspector, is due to it being a `UnityEvent`, which are `serializable`. UnityEvents, by default, have the return type `void` and take no dynamic parameters, as indicated by the empty brackets, though we will learn how to modify this, to allow UnityEvents with parameters.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca393202-9061-4e46-854b-1053cfda31db/Untitled.png)

![Static Parameters are possible, even for default UnityEvents.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cf456bb7-ef28-48cb-92ea-de8e083af363/Untitled.png)

Static Parameters are possible, even for default UnityEvents.

Example Source: [https://www.youtube.com/watch?v=TdiN18PR4zk](https://www.youtube.com/watch?v=TdiN18PR4zk)

```csharp
public class Player 
{
	void Die()
	{
		FindObjectOfType<Achievements>().OnPlayerDeath();
		FindObjectOfType<UserInterface>().OnPlayerDeath();
	}
}

public class Achievements 
{
	public void OnPlayerDeath() { }
}

public class UserInterface 
{
	public void OnPlayerDeath()
}
```

```csharp
public class Player 
{

	public delegate void DeathDelegate();
	public event DeathDelegate deathEvent;

	void Die()
	{
		deathEvent?.Invoke();
	}
}

public class Achievements 
{
	void Start() 
	{
		FindObjectOfType<Player>().deathEvent += OnPlayerDeath;
	}

	public void OnPlayerDeath() 
	{
		FindObjectOfType<Player>().deathEvent -= OnPlayerDeath
	}
}

public class UserInterface 
{
	void Start() 
	{
		FindObjectOfType<Player>().deathEvent += OnPlayerDeath;
	}

	public void OnPlayerDeath() 
	{
		FindObjectOfType<Player>().deathEvent -= OnPlayerDeath
	}
}
```