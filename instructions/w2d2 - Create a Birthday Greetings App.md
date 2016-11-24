![Makers Institute](../images/logo-makersinstitute.png)

# Create a Birthday Greetings App

## Objective
Practice Git Animation and Transition

In this project you will create a new Android Project, like previous one. Create some User Interface to say happy birthday to anyone, then add animation and transition to make a good User Experience.

Here are example of the final product [THIS](https://github.com/makersinstitute/android-academy/blob/master/etc/sample%20final.mp4). Good luck!

</br>

### First: Create a New Android Studio Project



### Second: Create a Layout
Here are an example how the layout might look like. You can have different image assets, text, and ImageButton's image. When applying image, try to configure the *android:scaleType* properties to make it looks nice on the screen. For ImageButton, you can grab the image from [here](https://materialdesignicons.com/).

![sample UI](../images/w2d2%20-%201.png)

</br>

### Third: Adding Animation
The reason the UI use the turbine image, because when user tap the *refresh image button*, the image will rotate on every tap. To add animation, take a look at the [Material Design #2](https://docs.google.com/presentation/d/1P2XoJXCsrHPaKYbafKewj40MLLuqPSjypUop14Le6I8/edit?usp=sharing) slide. The instructor will guide you through the process, in case you don't understand.

- ***Step 1***: Declare the variable in java files
- ***Step 2***: Connect the variable to the view
- ***Step 3***: Add the *setOnClickListener* method
- ***Step 4***: Define the *animation* function

You can also have a different animation with different image. There are several animation type available. Check the slides again for the list.

</br>

### Fourth: Running on Your Device

To see the results, try running the app on the device. There are information in ***Additional Material*** right [here](https://github.com/makersinstitute/android-academy#day-2---bold-graphic-design-meaningful-motion) to help you connect to the mobile devices.

</br>

### Fifth: Adding Transition
To add a transition effect, create another layout resources file. Copy paste the code from the first layout. But maybe remove some view and change into different position. Let the view's id (*android:id*) stays the same. For example, like this one

![second UI](../images/w2d2%20-%202.png)

The reason why there are no padding or margin to it's parent, because we are going to replace the previous ViewGroup, into this layout. So this layout will have the first layout's margin or padding.

- ***Step 1***: Create a new layout files
- ***Step 2***: Declare the variable in java files
- ***Step 3***: Connect the variable to the view
- ***Step 4***: Add transition's xml files (**take a look at the slides**)
- ***Step 4***: Add the *setOnClickListener* method into another ImageButton
- ***Step 5***: Define the *transition* function

</br>

### Sixth: Run Again on Your Device
It should be working by now. If not, try to ask the instructor or Google it by yourself.

</br>

### Seventh: Add Costumization
The app might look boring. Try to add themme, or styles. Maybe playing with some color contrast, [here](https://material.google.com/style/color.html#color-color-palette) between view. Make sure you follow some typography guidelines from Material Design, [here](https://material.google.com/style/typography.html) to let user feel comfortable.

# Congrats!
