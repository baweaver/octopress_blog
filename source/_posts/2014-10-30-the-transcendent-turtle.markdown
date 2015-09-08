---
layout: post
title: "The Transcendent Turtle"
date: 2014-10-30 20:16:31 -0700
comments: true
categories: [Minecraft, Lua, Turtle, Computercraft]
---

To many, Minecraft was a gateway drug to the world of technology. Redstone was a novel idea that let us experiment with some circuitry, make traps, and in general create more dynamic things. It's great for the basics, and a lot of fun to work with, but the interesting thing about it is that you're already starting to program by using it. Why not take it a step further? Computercraft gives you the power to jump into a full programming environment inside Minecraft using Lua.

<!-- more -->

## But I'm not a Programmer!

Neither are the people who frequently play Minecraft. Really, you don't even have to know how to program to get the benefits of the mod. Programmers are a peculiar breed who love to share their creations publicly. That means you can get some amazing tools and scripts from brilliant people simply by looking through the [Computercraft Forums](http://www.computercraft.info/forums2/)

Say you can't find it but you want to make something. There are [tons](https://www.youtube.com/watch?v=DSsx4VSe-Uk) [of](https://www.youtube.com/watch?v=bnKuOJOaWIA) [tutorials](https://www.youtube.com/watch?v=H5a7S4eF7zw) [out](https://www.youtube.com/watch?v=3zUEprIoFwA) [there](https://www.youtube.com/watch?v=1vK5rOkiW7g) for how to get started with computercraft.

## It's going to be hard!

If you already use Redstone, you're working with a lot harder material already. All those logic gates you use to get basic doors to work? What if you could just put a password on it? Simple:

```lua
-- Reference for more advanced: http://computercraft.info/wiki/Making_a_Password_Protected_Door

while true                            -- we want to keep the program going
  print("What is your quest?: ")       -- Give them a prompt to let them know what you want
  input = read("*")                   -- Read in their input
  if input == "holygrail" then        -- Is the input the password we want?
    redstone.setOutput("back", true)  -- Send a redstone current behind the computer
    sleep(2)                          -- Wait a few seconds
    redstone.setOutput("back", false) -- Lock it again
  end
end
```

No having to reference that long image about logic gates, that's it. Welcome to the concept of programming abstractions. Redstone was a low level language, and Lua is a lot higher level.

Surely those mining robots are harder to make though. Not really. Want to make it 

```lua
-- Reference for more advanced: http://pastebin.com/73gH7BUL

print("How far we going boss?: ") -- Ask them how far to go
distance = read("*")              -- Get the distance

for i = 0, distance do -- For the numbers 0 up to the distance that was entered...
  turtle.dig()     -- Dig in front
  turtle.forward() -- Move forward
  turtle.digUp()   -- Dig above
end  -- ...and repeat!
```

## I don't even know what they can do

That's what the [Wiki Pages](http://computercraft.info/wiki/Main_Page) are for! Tons of information on how turtles work, what commands they can run, and various other handy bits.

## Typing on that terminal is annoying

I agree, and I don't bother with it either. I type my code and post it on [Pastebin](http://pastebin.com/), and then just download it to the turtle like this:

```lua
-- From http://pastebin.com/73gH7BUL
pastebin get 73gH7BUL digger
```

...where [73gH7BUL](http://pastebin.com/73gH7BUL) is the url hash of the pastebin, and digger is the name we want to save the program as. All we need to do to use it now is to type in digger in the terminal and it's off on its merry way.

## It defeats the purpose of the game

It really depends on who you ask. To me, the purpose is to build cool things, not spend forever gathering the resources to make it happen. Computercraft allows you to automate a lot of that work, and the nice thing is that most of the scripts for common things like digging tunnels and stairs are already out there for you to use.

If you feel content spending hours on mangling redstone to do what you can do in under 20 lines of Lua in a few minutes, more power to you. Best hope you didn't make a mistake, or you'll end up digging the entire thing up again. To me, it enhances the game by allowing you to get more done faster.

## Tunnels? Lame.

How about a swarm of mining turtles controlled by a boss?: https://www.youtube.com/watch?v=g5153BiTNI8

3D Printing from a turtle GUI Paint program?: https://www.youtube.com/watch?v=AuofE9dqiuU

Youtube videos in Minecraft?: https://www.youtube.com/watch?v=tpqOv7SxkHA

Maybe a massive villager shopping mall: https://www.youtube.com/watch?v=Xasa_Jr-lcI

Though a Minecart Station may be your thing: https://www.youtube.com/watch?v=ws4iDwLc0zQ

The point is, if you can imagine it, someone has probably already built it. If not, you can make it. Of course the more advanced you get the harder it'll be, and programming can get hard past the trivial stuff. It takes time, but you can ask on the forums to get the help you need.

## It favors Programmers

Well, yeah, it is programming in Minecraft. Experienced programmers will have an edge. The good thing is that most programmers love sharing their toys, and love it even more when people use them and thank them for it. The thing to remember is that all of the seriously advanced programs out there take days to weeks to complete, so they're not getting an easier time necessarily.

There are already tons of scripts online of all types to download that will have you running at about the same level as any mid-range developer, and they're even documented. Even if there is a veteran on the server, chances are they like to share as well. Just ask some time.

## I don't want to have to redownload things

Yeah, me either. If you're sufficiently advanced you're going to run into the issue of remaking your programs and having different versions out there. There are a few of us out there crazy enough to try and fix that issue with an entire deployment management system for turtles like [Tortuga](https://github.com/baweaver/tortuga) (WIP) which will take care of a lot of that. Think Opscode Chef for turtles.
