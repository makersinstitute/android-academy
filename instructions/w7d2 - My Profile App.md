![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Week 7 - Day 2 (My Profile App)

#### Step 1: Create a New Project

#### Step 2: Create your Profile Activity

As your first activity to be shown, create an activity named MyProfileActivity or its okay to have it named as MainActivity. Make sure it has its own XML Layout.


#### Step 3: Design your Profile Activity

You are free to design your own imagination of a Profile Activity, but at least it has,
- `ImageView` for your profile picture
- `TextView` to show your name and your occupation
- `Button` for moving around the activity

Here are some example,
![my profile](../images/w7d2%20-%201.png)

#### Step 4: Connect your Design to the Code

Declare the `ImageView`, `TextView` and `Button` then connect it using `findViewById`.

#### Step 5: Using Camera

1. Request Camera Permission

```xml
<manifest ... >
    <uses-feature android:name="android.hardware.camera"
                  android:required="true" />
    ...
</manifest>
```

2. Add a Function to use Camera then Take a Picture

```java
static final int REQUEST_IMAGE_CAPTURE = 1;

private void dispatchTakePictureIntent() {
    Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE); // intent to camera
    if (takePictureIntent.resolveActivity(getPackageManager()) != null) { // check if camera is available
        startActivityForResult(takePictureIntent, REQUEST_IMAGE_CAPTURE); // running the intent
    }
}
```

3. Getting the Camera Result

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == REQUEST_IMAGE_CAPTURE && resultCode == RESULT_OK) { // check if the request code is from camera and if the result is ok
        Bundle extras = data.getExtras(); // getting data from the camera
        Bitmap imageBitmap = (Bitmap) extras.get("data"); // assign the data, which is a picture to a variable
        mImageView.setImageBitmap(imageBitmap); // applying the picture into an ImageView
    }
}
```

4. Add a `ClickListener` to the `Take Picture` `Button` which calls the `dispatchTakePictureIntent()`

#### Step 6: Create a New Activity for Editing your Profile

1. Just create an Empty Activity and give a proper name, for example, `EditProfileActivity`. Then design the layout which contain two `EditText` then a `Button`.

Here some example,
![edit profile](../images/w7d2%20-%202.png)

2. Connect the Layout to the Java Class



#### Step 7: Add a ClickListener on `EditProfileButton`

Since we are going to have a two-way comunication, starting an activity and receiving a result, there are some differences.

1. Specify a Request Code

We are have another request code for camera, so declare another code to differentiate `static final int REQUEST_POFILE_DETAILS = 11;`

2. Declare the `Intent`

`Intent intent = new Intent(MainActivity.this,EditProfileActivity.class);`

3. Declare the `startForActivityResult()`

`startActivityForResult(intent, REQUEST_POFILE_DETAILS);`

#### Step 8: Create a Return Statement inside the `EditProfileActivity`

To return to the previous Activity, we are going to add some method inside the `Save`'s `ClickListener`. Add the following code.

1. Declare the `Intent`
A normal `Intent`, `Intent intent = new Intent();`

2. Pass the data from `EditText` using `Intent`'s `putExtra`
The first parameter is the key, and the second is the value.
```java
intent.putExtra("name", nameEditText.getText().toString());
intent.putExtra("occupation", occupationEditText.getText().toString());
```

3. Setting the Activity Result
The first parameter is the requestCode, and the second is the `Intent`.
`setResult(REQUEST_POFILE_DETAILS, intent);`

4. Finish the Activity
Just type `finish();` so the `EditProfileActivity` will exit and shows the `MainActivity`.


#### Step 9: Receiving the Activity Result

Previously we are already have an `@Override` method that override the `onActivityResult()`, so for now we are just going to add another `if/else` statement to check where the result came from.

```java
else if (requestCode==codeRequest) {
             name = data.getStringExtra("name");
             occupation = data.getStringExtra("occupation");
             
             nameTextView.setText(name);
             occupationTextView.setText(institute);
         }
```

#### Step 10: Saving Name and Occupation's text

It's not likely to be a normal behaviour if everytime we open the app, our profile is missing. So we are going to save the profile's name and profile's occupation to some place, and we call it, `SharedPreference`. Where should we put the code? Well, to make sure everything is saved, we will try to save the data on the application paused, so we Override `onPause()` method and put some `SharedPreference`'s code.

Here are how we do it,

1. Create a SharedPreference variable
`SharedPreferences mSharedPreferences = getSharedPreferences("pref" , MODE_PRIVATE);`

2. Create an Editor of SharedPreference variable
`SharedPreferences.Editor mEditor = mSharedPreferences.edit();`
3. Put the data we want to save
```java
mEditor.putString("name", nameTextView.getText().toString());
mEditor.putString("occupation", occupationTextView.getText().toString());
```
4. Save it.
`mEditor.apply();`

Then, once we open the app again, method inside `onCreate()` and put another `SharedPreference`'s code.

1. Create a SharedPreference variable
`SharedPreferences mSharedPreferences = getSharedPreferences("pref" , MODE_PRIVATE);`

2. Get the data we want to show
```java
String name = mSharedPreferences.getString(Name,"");
String occupation = mSharedPreferences.getString(Institute,"");
```
3. Display it to the view
```java
nameTextView.setText(name);
occupationTextView.setText(occupation);
```


# Congrats!




