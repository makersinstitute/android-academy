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

Once you create from a template, you will have a `Java Class` and  `XML files`. Design a good layout that represent what can be done on that screen. Just create a simple one.

#### 3. Access Created Views on XML inside the Java Class file

For example, `TextView songText = (TextView) findViewById(R.id.song_name);` . Make sure you attach all of them.

For Play Button and two others, try to use `ImageButton` with the image source came from Android `android:src="@android:drawable/ic_media_play"`

Also, add a representative image to be shown on "Now Playing" screen.

#### 4. Find a Simple Audio Files on the Internet

It's a Music app. So at least we have to hear something from it.

#### 5. Add the Audio Files to Project

Under the `res` folder, right-click, `New` -> `Android resource directory`. Choose `raw` as the resource type. Then add the audio files inside that folder. By drag and dropping.

#### 6. Done with the Extras, Let's Code

Before we continue, make sure you have:
1. Three fragment files with its XML files, named `fragment_xxx.xml`,
2. Three model files,
3. The `MainActivity` class with its XML files, named `activity_main.xml`,
2. (At Least) Three new activity files with its XML files,
3. Audio files inside `res/raw/`


#### 7. Add an Intent for each Element on each Tabs

Here, you maybe don't understand what fragment is. For now, fragment is a part of your layout, which holds by it's parent. So, by having three tabs, it means, we have three fragment (you can also re-use one fragment for all of them). Each fragment have it's own class, and its layout, that's why we have three pair of them on the project.

Now, open the `SongFragment` class. We want to have a new page open by tapping the Song Name on the screen, so below `listView.setAdapter(mAdapter);` we add a `clickListener` like this

```
listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
                Intent intent = new Intent(getActivity(), ViewAlbumActivity.class); // Define the target activity that we want to open
                intent.putExtra("album", items.get(i).getName()); // Passing some arguments to target activity
                intent.putExtra("artist", items.get(i).getArtist()); // putExtra take two parameters
                intent.putExtra("song", items.get(i).getSong()); // key name, and the value
                startActivity(intent); // and we start the process here, open a new Class by also giving the extra information
            }
        });
```

If you are curious what `items.get(i).getName()` is, the `items` is an array of items, for this page is `song` item. Then, the `get(i)` is like accessing the array, the item number `i`. And `getName()` is part of the getter and setter inside the model files. To get the name that has been declared on `Song song1 = new Song("Air Hujan", "Trio Kwek Kwek", "MKKB");`

#### 8. Receive the Information on The Target Class

Here the example code, `String songName = getIntent().getStringExtra("song");`. First, declare the dataType, variableName, then get the `Intent` that has been sent from the previous `Activity`. Then followed by `getStringExtra()` method, with the key name as its parameter.

#### 9. Use the Received Extras to Somewhere

For example, since we already declared the `findViewById`, now we just assign the string to the view, `songText.setText(songName);`

#### 10. Set Click Listener if you Have a Button that Acts as a "Door" to another View/Class

For example, 
```
playButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(ViewAlbumActivity.this, NowPlayingActivity.class);
                intent.putExtra("artist", artistName);
                intent.putExtra("song", songName);
                intent.putExtra("album", albumName);
                startActivity(intent);
            }
        });
```

#### 11. Now Playing Screen!

From the starter code, we have three songs, so at least we have three audio files. Then, how to make the now playing, plays a right song? We have to declare a conditional statement. You can use `if/else` or `switch/case`. For now, let's use `switch/case`. 

But before we continue, in this page, we want to able to play the audio files. So we need to know how to do it. We will use `MediaPlayer` as the audio player. Declare this, `MediaPlayer mp;` . Inside that class, there are a lot of things, but we just use `start()`, `stop()`, and `pause()`. Just type `mp = MediaPlayer.create(NowPlayingActivity.this, R.raw.ujan);` then `mp.start();`. Voila, the audio files will be played.

Back to the `switch/case`, to differentiate what should be played, we take the `songName` arguments as the switch. Then declare every possibilities. Don't forget to put the `switch/case` under `playButton`'s `clickListener`

```
switch (songName) {
            case "Air Hujan":
                playButton.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        mp = MediaPlayer.create(NowPlayingActivity.this, R.raw.ujan); // create(whereToPlayTheAudio.class, theAudioFileLocation)
                        mp.start();
                    }
                });
                break;
        .
        .
        . // another case should be added here
```

#### 12. Add another Click Listener for Pause and Stop

```
pauseButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                mp.pause();
            }
        });
```

#### 13. Last One!

To let you able navigate back and forth, we edit some code inside the `AndroidManifest.xml` file. When you `Intent` to another class, then want to go back to the first location, we can just add another `Intent`, but here are some simple way.

Add, `android:parentActivityName=".activity.MainActivity"` inside the activity brackets

```
<activity android:name=".activity.NowPlayingActivity"
            android:parentActivityName=".activity.MainActivity"/>
```

You can have different `parentActivityName`, it all depends on you.


#### 14. Congratulations~
