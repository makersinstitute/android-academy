![Makers Institute](../images/logo-makersinstitute.png)

# Adaptive Design: Contact App

![Contact App](../images/w2d3%20-%201.jpg)

## Objective
Practice Adaptive Design for potrait and landscape view on mobile device.

In this project you will create a Contact App that will have a different layout on different phone orientation. By giving some configuration on layout resources. Since one of it's feature only work on API 21 or higher, you will have to import a module on your build.gradle, the instructors will help you solve this issues. **Let's get started!**

</br>
### First: Create a New Android Project

### Second: Create a New Layout with GridLayout Template
Inside GridLayout you will have several contact information of your bootcamp's friend. Take a picture of your friend, ask his/her name, also his/her phone number. Then create a RelativeLayout, containing Image, Name, Phone Number, and a Button. Repeating creating that RelativeLayout  as many as your friend.

</br>
### Third: Nest the GridLayout inside ScrollView

### Fourth: Include the GridLayout Layout to activity_main.xml
Using `<include layout="@layout/grid_layout_name"/>`

### Fourth: Create an Integer Resources File for Adaptive Function
Under *res/values/integers.xml* and *res/values-land/integers.xml* create a new integer number with different number. `<integer name="column_count">3</integer>`

</br>
### Fifth: Configure the GridLayout Using the integer.xml File
Add this properties inside GridLayout `app:columnCount="@integer/column_count">`

</br>
### Sixth: Run on Your Device


# Congrats!
