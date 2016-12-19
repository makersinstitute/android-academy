![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Instructions for Week 6 - Day 1 (Music App)

#### Step 1, Clone the Starter Code [here](https://github.com/makersinstitute/MusicApp). 
If you want to start the project from scratch, start new project with Tabbed Activity with Navigation Style: Action Bar Tabs (with View Pager)

#### Step 2, Build the Projects

#### Step 3, Imagine the App Usage Flow
What should happen if I click this, or where should you navigate from this page, or else. Try the final app that has been shared from your instructor. Explore the whole thing and try to understand what really happens behind the scene.

#### Step 4, Let's Code!

=====================================

### Coding Guide

#### 1. Create Several Activity Template

As you can see, the app has three tabs, with name "By Songs", "By Artists", and "By Albums". So at least we should make at least three activity to handle every action on each tabs.

#### 2. Design a Proper Layout

Once you create from a template, you will have a `Java Class` and  `XML files`. Design a good layout that represent what can be done on that screen. Just create a simle

#### 3. Access Created Views on XML inside the Java Class file

For example, `TextView songText = (TextView) findViewById(R.id.song_name);` . Make sure you attach all of them.

For Play Button and two others, try to use `ImageButton` with the image source came from Android `android:src="@android:drawable/ic_media_play"`

Also, add a representative image to be shown on "Now Playing" screen.

#### 4. Find a Simple Audio Files on the Internet

It's a Music app. So at least we have to hear something from it.

#### 5. Add the Audio Files to Project

Under the `res` folder, right-click, `New` -> `Android resource directory`. Choose `raw` as the resource type.
