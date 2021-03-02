---
title: Basics of Game Engine Development
date: 2021-03-02 16:00:00 +0100
category: Game Engine
---

In case you have read the [introductory article]({% post_url 2021-02-20-Introducing %}), you should know that this blog is partly about game engine development.
And in case you haven't, I suggest you to read it. This first of many to come articles about game engine development will introduce the subject :
what is a game engine, How to make such a project, What does it take... In short, where to start.

## Foreword

Before we dive right into the subject, I would like to remember you that **I'm no professional**. I am not officially from the video game industry and I only
spend my spare time on programming and game development. This blog is about sharing the knowledge and experience I've acquired over the years about programming
and game engine development, and this is why I will try to link to my articles as many resources on the subject as possible. Thus I strongly encourage you to
take a look at these resources, find more and further your own experience. I only want to share my experience but some of what I will show may not fit your needs or be optimal.

By the way, you can give me feedback in the comment section, at the bottom of this article 👍.

Now, let's dive into the subject.

## What is a Game Engine ?

If you are here, it's probably because you are interested in game engine development. But do you even know what this is ?
So let me take you through the basics to clarify this point.

### Engine or Framework ?

There is plenty of different definitions of game engines across the Internet, and you probably heard about Frameworks. Indeed, frameworks, engines and even library are quite similar things. After all,
these are just collections of functions tailor-made for a specific need, in other words, powerful tools designed to make a programmer's life easier : a library is a bunch of functions used, for instance,
for logging, memory management or graphic card interfacing _(e.g OpenGL, DirectX)_; a framework is a bunch of libraries; and an engine is a bunch of frameworks... Ish.

This a quite simplistic definition but close enough to reality. From a technical point of view, the difference between these is a matter of **levels of abstraction**. Long story short, the level of abstraction
of a program correspond to its level of specialisation, but I highly recommend you to read [this excellent article on the subject](https://www.fluentcpp.com/2016/12/15/respect-levels-of-abstraction/) as levels of abstraction is, in my opinion, fundamental to understand. For exemple, a logging library is at a fairly low level of abstraction : you can do about anything with it as long as it implies logging.
But a multimedia framework, like SDL, is designed to make graphical applications and, more commonly, video games. Thus it is at a much higher level of abstraction.

A game engine is theoretically at a much, much higher level of abstraction _(much)_. In a perfect world _(which doesn't exist)_, an industrial strength game engine should allow us to make games without a single line
of code ! It is almost at an "application" level of abstraction, which means that it is not just a bunch of lines of code but almost an application you can execute on your computer !
In the end, almost all you have to do to make a game with it is the "assets" : sounds, musics, textures, maps, 3D models, sprites, animations... Take a look at industrial strength commercial game engines like
Unity or Unreal Engine and you will see !

## Complexity

Now, maybe you are thinking "This is too difficult, I will never be able to make a game engine like Unreal alone !", and no one ask you this !

The complexity of your game engine really depends on you and what you want : typically, many Game Jam developers who are not using any game engine _(like Unity or Godot)_, develop their own "simplified"
version of Game Jam engines in a week or two, commonly based on other multimedia frameworks _(like SFML or SDL)_, and this is not rocket science !
On the other side, some developers wants to go further and make _(alone or in small team)_ "studio quality" game engines... But it takes more time though.

It also depends on the choice of dependencies : by using libraries and frameworks _(for mathematics, windows and I/O management, play sounds, open images, memory management... And the list goes on)_
you don't have to worry about low level routines and ensure you are using reliable and efficient code. Nevertheless, you should be careful when using dependencies, if certain ones can actually improve
the overall quality of the final product, too much dependencies can easily make your code hard to maintain and difficult to port if you are not careful enough. Moreover, setting up a development environnement
_(for you or another new developer arriving on your project)_ become increasingly difficult with the number of dependency. You sometimes even have to rework certain dependencies, for better performance for instance
_(e.g [EASTL](https://github.com/electronicarts/EASTL), the optimised STL from EA (thanks to Bm from Arcadia Corp for this one))_. There is much more to say on this subject, and we'll come back to it later.

For this series, we are going to develop a "studio quality" engine, this means we are not expected to make a tool as big and functional as Unity, for exemple, but an engine capable of making an indie
commercial game, that an indie game studio could be using. _(By the way, did you know that I'm an indie developer ? That is why I make indie game engines for indie game studios 😄)_

So let's see what we expect from our game engine.

## Anatomy

Indeed, a game engine is a fairly complex thing _(and I don't necessarily mean by that "Difficult", just the opposite of "Primitive")_, it is made of numerous sub-systems and various modules,
typically, a graphic, physic, audio, and perhaps a network engine but also lots of debugging tools, a resource manager, memory manager etc. to make our life easier
and boost the reliability and maintainability of our engine.

### The graphic engine

This is where you will find the graphic card interface and all the necessary to draw almost anything on the screen in a simple way. It is generally based on OpenGL, DirectX or Vulkan, which are the standard libraries
for this task and whose implementations are packed with you graphic card drivers : so remember to update them frequently !

This is usually one of the biggest sub-system and developers tend to focus on it more than anything else, but there are other big sub-systems as well. Indeed, many tutorials on Internet essentially focus on the graphic engine, and there is a lot to say about it, this is true, but I want to bring balance back and focus on other important things. Thus, we will speak about the graphic engine much later on this series.

It can be simple as it can be complicate, 2D, 3D, or both, require mathematical knowledge, mainly [linear algebra](https://www.youtube.com/playlist?list=PL0-GT3co4r2y2YErbmuJw2L5tW4Ew2O5B), or not, and may need
to communicate with the physic engine. Which gives me a nice transition.

### The physic engine

Another large piece of the engine. Unsurprisingly, this system will be in charge of everything related to physics : collisions, forces, solids etc. That being said, many other technics and technologies may
come into play and require physical knowledge. There is lots of resources available on the Internet on the subject and you can even directly go for a degree in physics _(like me 😄)_ but if this scares you,
you can totally work with existing physic engine like Box2D _(for... 2D)_ or PhysX by NVidia, which is free.

### Playing audio

In the same way, audio engine can be very simple or terribly complicated. Opening different types of audio files _(mp3, ogg, wav...)_ and the need for multi-threading to play them
_(or system interruptions, for the nostalgic ones)_ and I'm not even speaking about sound manipulation routines _(reverb, distortion...)_ will probably make us use
reliable and powerful third-party libraries like OpenAL, FMOD, or Wwise.

### Networking

Network support can be optional as your games can totally be offline. But if this is not the case, making a network engine is another area of expertise, and can be again simplified by the use
of third-party libraries. Note that it could be interesting to wait for C++23 _(this means 2023)_ for the networking to be _(possibly)_ standardised.

### Graphical interface

Most modern engines comes with a shiny graphical interface that makes them looks like simple multimedia designing tools more than actual game engines. This is something we might want for our engine in
the future : an easy to use graphical interface, that could allow us to make games without a single line of code.

### The CORE

This is the part of the engine with the lowest level of abstraction. It contains all the basic tools and systems, as well as the interface between the operating system and the rest of the engine called
the "Platform Independance Layer" that will allow us to make our engine cross-platform easily. Typically, we expect it to support (among other things) :

* Logging
* Memory Management
* Resource/assets management
* Time
* Math
* Dynamic libraries management (like DLLs)
* An Event system
* Lots of debugging tools
* Internationalisation _(Or I18n)_
* RTTI & serialisation
* File management _(CSV, XML, YAML...)_
* Random number generator
* Exception handling
* Scripting
* ...

And potentially a lot, lot more, depending on the level of complexity you are aiming at, like collections, threading, video playing, signals, etc. You may or may not implement each of these in your engine,
it all comes down to you and your preferences.

In a nutshell, there is not much chance that this part of the engine will be very visible in your future games from the outside, but this is where, in my opinion, all the magic happens.
A core system and tools well designed and architectured are, still in my opinion, what determines the reliability, efficiency and evolutivity  of your engine :
if while developing a game using your engine you notice an unexpected weakness in these core
systems, forcing you to rework all their design, you will lost precious time and motivation. You don't want that, so it is important to spend time designing these systems carefully.

## Tips & tricks

In the end, making a game engine is a huge project. A massive one. And this is a sufficient reason not to just dive into code and start developing it with no **prior preparation**. It is important in the first place
to spend time studying the problem, designing the engine and testing it. You could take a look at what has already been done : there are plenty of games, engines and frameworks whose code are available online
_(take a look at id Software's github for exemple)_, this way, you will get a better understanding of how it works, what you might want or don't want to be in your engine and what are the
best design solutions for it.

Finally, when you will start having a decent idea of where you are going and feel ready to code, I can recommend you to **know your language** sufficiently well : in my case, I'm planning to use C++. A good understanding
of how C++ works, Object Oriented Programming, memory, the standard library and its algorithms and, optionally, template meta-programming, will be needed in order to make a robust and reliable engine.
I will also grant special value to **code expressiveness** : there is nothing worse than not understand anymore a piece of code you wrote a few days/hours earlier. The objective is to make a clearer and cleaner code
that you or another developer can easily understand, thus improving code maintainability. For this, I will make heavy use of the articles found on [Fluent C++](https://www.fluentcpp.com).

Lastly, it is essential to know your **design patterns** _(thanks to Kern, creator of [Kalaxia](https://kalaxia.org/), for this advice)_ as well as the patterns that can be related to your programming language
_(like C++20 "Concepts")_.

## Useful resources

To conclude this introduction, I suggest you a non-exhaustive list of resources I gathered along the years _(Youtube channels, Blogs, Books...)_ about game engines, or just resources that you might
find useful on your journey to game engine development _(at least, I have)_. Still, you might stumble upon many other resources like these so remember to leave a comment with a reference
and I'll be glad to add them to this list.

### Youtube channels

I suggest you to take a look at these channels' playlists.

* [The Cherno](https://www.youtube.com/c/TheChernoProject), many resources and tutorials about C++ and game engine development.
* [3Blue1Brown](https://www.youtube.com/channel/UCYO_jab_esuFRV4b17AJtAw), Math, linear algebra, calculus and more, if you are curious.
* [thebennybox](https://www.youtube.com/user/thebennybox), tutorials and videos about game development, computer graphics and game engine development.
* [Hopson](https://www.youtube.com/c/Hopsonn), a few tutorials and many game projects, if you want to discover new things and read code on his github.
* [ChiliTomatoNoodle](https://www.youtube.com/c/ChiliTomatoNoodle), a few more tutorials on C++, programming and computer graphics (notably DirectX).

### Web sites

* [Fluent C++](https://www.fluentcpp.com), the excellent blog of Jonathan Boccara about C++, recent C++ news and above all **code expressiveness**.
* [Learn OpenGL](https://learnopengl.com/), if you want to learn computer graphics and OpenGL.
* [Open.gl](https://open.gl/), more on OpenGL.
* [Lighthouse 3D](https://www.lighthouse3d.com/), 3D graphics tutorials, mostly OpenGL and GLSL.
* [Codeproject](https://codeproject.com/), articles and code snippets for many uses.
* [Stack Overflow](https://stackoverflow.com/), the developer's choice _(essential)_.
* [Game Dev](https://gamedev.net/), forums and tutorials about game development.

### Books

* Game Engine Architecture (Third Edition), Jason Gregory
* 3D Game Engine Programming, Stefan Zerbst
* Head First Design Patterns, Eric Freeman & Elisabeth Robson
* 97 Things Every Programmer Should Know, Kevlin Henney
* The Art of Readable Code, Dustin Boswell & Trevor Foucher
* Becoming a Better Programmer, Pete Goodliffe

## Conclusion

I hope this introduction helped you get a clearer picture of the basic principles of game engine development as well as it gave you things to read/watch/think about. Like I said before, if you want to go further,
I recommend you to read code on the subject and try making some prototypes, see where the weaknesses and strengths of your approach are : this is what Game Jams are here for !

Finally, I am open to any constructive criticism, so remember to leave a comment !

Next time, we will speak about architecture, so stay tuned !
