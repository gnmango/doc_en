# Course Introduction
It is important to fully understand the concepts of frequently used models.
# Creating a Desired Entity
To create a desired entity, you need to add components to the entity and modify its properties.

Let's make a tree like so.
![Model_1](https://mod-file.dn.nexoncdn.co.kr/bbs/1634553528053ca48e93975e04ccaa843244bbcde7219.png "Model_1")
<br>
We created an entity, added components, and modified properties appropriately.
It's done!
<br>
Now, we want to gather some trees to make a forest. What should we do?
![Model_2](https://mod-file.dn.nexoncdn.co.kr/bbs/16345535759657d1c01e5106d4d939c547586dae942b3.png "Model_2")
<br>
You just create an entity, add components, and modify properties four times.
But what if there are 100 trees?
That's right. We have to do it 100 times.
This is way too much trouble. What should we do instead, then?
# Model
Let's think back to daily life for a moment, and imagine that we are painting a picture.
![star_circle](https://mod-file.dn.nexoncdn.co.kr/bbs/1635480499353135233e3daf946998fda9453abc5312b.png "star_circle")
<br>
Let's draw a star like this.
Assuming you're drawing multiple stars, what's the easiest way to do it?
There are many ways to go about it, but as a representative example, you can make a stamp like this.
![stamp_starcircle](https://mod-file.dn.nexoncdn.co.kr/bbs/163548052969636561321444d4686876e3d3b69442c8f.png "stamp_starcircle")
<br>
If you stamp this, you get stars, so you can get 3 stars with just 3 stamps.
![star_circle](https://mod-file.dn.nexoncdn.co.kr/bbs/1635480499353135233e3daf946998fda9453abc5312b.png "star_circle") ![star_circle](https://mod-file.dn.nexoncdn.co.kr/bbs/1635480499353135233e3daf946998fda9453abc5312b.png "star_circle") ![star_circle](https://mod-file.dn.nexoncdn.co.kr/bbs/1635480499353135233e3daf946998fda9453abc5312b.png "star_circle")
<br>
Let's go back to MapleStory Worlds.
It would be convenient to have such a "stamp" in MapleStory Worlds.
That's exactly what a **model** is.
<br>
A **model** is a version that contains information about the components and properties of an entity.
You are already very familiar with this particular model.
![Model_3](https://mod-file.dn.nexoncdn.co.kr/bbs/165993039740743e32904b0f442ebbd627968576bb02a.png "Model_3")
<br>
The **Preset List** provides basic presets based on MapleStory data.
So, what if you need to change components and properties in MapleStory resources and use them frequently? Or, what if you need to create your own completely new entities in the world you're building and use them frequently?
# Modelization
Let's go back to our daily life for a moment.
![star_circle](https://mod-file.dn.nexoncdn.co.kr/bbs/1635480499353135233e3daf946998fda9453abc5312b.png "star_circle")
<br>
You made a stamp like this, but you don't like the round border.
We want to switch to a square frame. So, we erase the circle and draw a square.
![star_quadangle](https://mod-file.dn.nexoncdn.co.kr/bbs/1635480621736d7207f056a4945c0bf13a531fb8a5e3f.png "star_quadangle")
<br>
You like it.
What should we do if we want to line up several of these stars in a row?
Stamp → Erase the round circle → Draw the square as many as the number of stars.
This is way too much. Is there any other easier way?
<br>
Yes, there is. We can make a new stamp.
![stamp_starQuadangle](https://mod-file.dn.nexoncdn.co.kr/bbs/1635480687931bb4a27c2a1154b239d9256faec98820f.png "stamp_starQuadangle")
<br>
Like this.
However, making a stamp isn't an easy task.
So what should we do then?
We can make separate frames for the star, circular frame, and square frame.

* Star stamp + circular frame = Star in circle
* Star stamp + square frame = Star in square

However, what if we need a triangular frame? What if we want to make a heart instead of a star?
You can combine them, but you have to divide them in an appropriate way. If a completely new type comes out, you have to create it from scratch.
The fundamental problem is that making a stamp isn't easy.
<br>
Let's go back to MapleStory Worlds.
In MapleStory Worlds, it is very easy to create a model (e.g. a stamp).
That's because we can take a drawing and turn it directly into a stamp.
<br>
In other words, converting an entity into a model, that is **modelization**.
Let's look at an example.
<br>
You created a talking tree by adding a **ChatBalloonComponent** to an existing tree model.
![Model_4](https://mod-file.dn.nexoncdn.co.kr/bbs/16345560690347d9b46707db345c8abf5b57f0cca67e9.png "Model_4")
<br>
To create 100 talking trees, you have to repeat placing the tree object and adding the **ChatBalloonComponent**.
There is a very easy way to avoid this simple repetition, and that is to **modelize** the first created talking tree. 
In the context menu of the talking tree, you can make an Entity into a model by clicking **Create Model From Entity**.
![5](https://mod-file.dn.nexoncdn.co.kr/bbs/1659932995539100fa46c3c824656b22c4ad30f184791.png "5")
<br>
You can check the created model in **Workspace - MyDesk**. Let's make full use of your own talking tree stamp.
![6](https://mod-file.dn.nexoncdn.co.kr/bbs/1659933437521b3f0c8b795714806b51ec2ca7d38689f.png "6")
