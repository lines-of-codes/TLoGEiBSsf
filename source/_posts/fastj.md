---
title: FastJ - A fast java game engine
tags: [fastj]
---

FastJ is a free and open-source Java-based 2D game engine and framework. It aims to provide the best 2D game-making experience using Java (and the JVM languages).

## Pros and Cons
### Pros
- Easy to use
- Uses minimal external dependencies (4 direct dependencies at the time of writing)
- Has a good relationship with your FPS
### Cons
- The project is dead (at the moment of writing)
- Only support Desktop platforms

## Try out FastJ
**This article assumes you already have the basic understanding of the Java programming language.**

To try out how FastJ works as quickly as possible, Fork [this replit template](https://replit.com/@LinesOfCodes/FastJ-Java-Gradle).

Or if you prefer to setup all the tools and your project on your PC, you can follow [this official guide](https://fastj.tech/wiki/fastj-basics/fastj-quick-start/) to quickly get started on setting up your project in a standard way.

## Let's do some explaining
Alright, So you might be curious about what all the code stuff really means.

So, First, the App (or Game) class extends a `SimpleManager` class.
What does the `SimpleManager` class do? It simply allows you to script in the logic of your game.

The `SimpleManager` class is essentially for coding small games where only one scene is nescessary.
Which is perfect for beginners to FastJ to be able to create small games easily, and quickly without all the 
hassle of managing scenes.

But what about when you want to step up into creating a more complex game? We'll talk about that in the [next section](#Creating-a-more-complex-game).

But for now, let's explain the rest of the code.

Let's jump to the main function first.
Now, What `FastJEngine.init("Hello", new App());` does, is it basically just start the engine with the specified window title (1st argument) and the game class, which will be either a class that extends `SimpleManager` or `SceneManager`.

Then you did some additional configuration and run the game by calling `FastJEngine.run()`!

Those additional configuration that is in between the init function call and the run function call, 
Usually can be resolution changes, or hardware acceleration configuration.

To change the resolution, There is 2 seperate methods to change the resolution.
- `FastJEngine.setWindowResolution` \- Sets the window size according to the resolution provided.
- `FastJEngine.setCanvasResolution` \- Sets the canvas resolution, the window's size isn't going to change when this is called.

So what's the real difference? I mean I can just use the `setWindowResolution` method right??? 
Unfortunately not, The `setWindowResolution` method only changes the size of the window but does not change the size of the game canvas. So for example, if you have used the `setWindowResolution` method to set the window size to a resolution with 16:9 ratio, but you used the `setCanvasResolution` method to set the canvas resolution to a resolution that has a ratio of 9:16, you would see clearly why it's different.

It's going to be stretched. Because the canvas is trying to scale to fill the window which does not have the same resolution ratio. This is called stretch-to-fill. This technique can also be used for pixel art games to allow using a technique called upscaling, as long as you got the resolution ratio and stuff right it's not going to be stretched.

To change the hardware acceleration settings, you actually don't need to do this but,
you can.
Anyway, you can set the hardware acceleration settings by calling the function `FastJEngine.configureHardwareAcceleration(HWAccel)`.
Which `HWAccel` can be one of the following options:
- `HWAccel.Direct3D` \- A windows specific graphics API.
- `HWAccel.X11` \- A Linux specific graphics API.
- `HWAccel.OpenGL` \- A Graphics API that most devices support.
- `HWAccel.CpuRender` \- Uses CPU to draw stuff. Highly does not recommend especially with complex scenes with lot of objects.
- `HWAccel.Default` \- Just use whatever the OS picks for me.

## Creating a more complex game
To create a more complex game, you will need multiple scenes, right?

So how to do manage multiple scenes in this game engine?

`SceneManager` is here to save the day.

First, Make a class that inherits from `SceneManager` (`tech.fastj.systems.control.SceneManager`) and override the `init` method.

Like so:
```java
class MySceneManager extends SceneManager {
    @Override
    public void init(FastJCanvas display) {
        // configure your display settings here
    }
}
```

Then, we'll create the scenes we want to manage. You can create a scene by creating a class and inheriting the `Scene` (`tech.fastj.systems.control.Scene`) class.

```java
public class MyScene extends Scene {
    @Override
    public void load(FastJCanvas canvas) {
        // When the scene is first loaded
    }

    @Override
    public void update(FastJCanvas canvas) {
        // Runs every frame when the scene is still loaded
    }

    @Override
    public void unload(FastJCanvas canvas) {
        // When the scene is unloaded
    }
}
```

Now you can add a text to render or some kind of shape to the scene class.

Make sure that you have a proper entry point and initialize the FastJ engine.
If you try to run it, nothing will happen.

Because you haven't added the scene to the `SceneManager` class.
You can do that by calling the `this.addScene(Scene)` method from inside the `init` method inside the inherited `SceneManager` class.

Then after you add it, you'll need to call `this.setCurrentScene(Scene)` and `this.loadCurrentScene()`.
And then you're done! Everything should work now unless you did something wrong.
