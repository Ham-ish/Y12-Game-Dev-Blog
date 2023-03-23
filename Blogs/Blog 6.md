# Game Dev Blog 

## 6/3/23: Revise Past Mistakes

### Overveiw

I was indeed swamped by the number of assignments coming up, ergo, this is the late blog indeed. mostly did robotics in the IT lessons. It occured to me that I just put in a bunch of pictures into last weeks blog, so I am going to explain them to prove that I have indeed, learned anything

### Current Goal

But first,

I did talk in class this week about my current pathway in game dev, for which the outcome is making a game. This means I will need to come up with an idea for a game and focus my learning around that idea. In my next blogs I plan to write down some of the better ones.

### Unreal engine textures more detail

The time that I spent learning Unreal taught me some material and texture creation. 

Here is all of the stuff I learned with explanations of how it works

<img src="../Images/Unreal engine 1.png" title="Unreal engine 1.png" width="800"/>

This is a texture containing an earth texture and a normal map. these are going into the colour and normal outputs of the material, meaning that if this material is applied to a sphere, like below, it wil look like the earth.

<img src="../Images/Unreal engine 2.png" title="Unreal engine 2.png" width="800"/>

Previous texture on a sphere

<img src="../Images/Unreal engine 8.png" title="Unreal engine 9.png" width="800"/>

This texture uses prelin noise in the metalic value to create a material that is always the same colour, but has different ammounts of shininess in different places


<img src="../Images/Unreal engine 9.png" title="Unreal engine 5.png" width="800"/>

This texture also uses perlin noise but it does not just do it in the metalic output. The texture interpolates two other textures, of moss and concrete bricks and combines them in the same places to make shiny concrete and dull moss

<img src="../Images/Unreal engine 7.png" title="Unreal engine 4.png" width="800"/>

Application of mossy concrete material

<img src="../Images/Unreal engine 4.png" title="Unreal engine 3.png" width="800"/>

This was the texture testing model used in the tutorial it has multiple surfaces that can have multiple textures, so I made the outer texture mossy concrete and the inner one a mottled black

<img src="../Images/Unreal engine 6.png" title="Unreal engine 6.png" width="800"/>

This is a texture that uses prebuilt normal and roughness texures that fit the colour so that the images can be put together in a material and fit perfectly. It also has a hue modifier, which changes the colour of the bricks from an orangey brown to the red wine colour that it outputs. Finally, the size is being modified by the three tiles on the left, where the botom one is a parameter, which means an instance of this material can be modified live, in the world, rather than having to edit the material itself. 

<img src="../Images/Unreal engine 5.png" title="Unreal engine 7.png" width="800"/>

Material application on sphere

<img src="../Images/Unreal engine 3.png" title="Unreal engine 8.png" width="800"/>

Material original next to material instance, and editing the instance size

### In Conclusion and plan for the next week

Still did not finish the Tutorial. So I shall continue chipping away at that. The reason I haven't finished it is that, when I get around to working on it, after about  20 to 40 minutes I'll get distracted and work on somthing else.

With this in mind next weeks goal is still to finish the tutorial with a renewed vigor, and also to come up with good ideas for games to create.