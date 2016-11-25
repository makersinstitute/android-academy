![Makers Institute](../images/logo-makersinstitute.png)

# Adaptive Design: Contact App

![Contact App](../images/w2d3%20-%201.jpg)

## Objective

</br>

### First: Create a New Android Project

</br>

### Second: Create a New Layout with GridLayout Template
Inside GridLayout you will have several contact information of your bootcamp's friend. Take a picture of your friend, ask his/her name, also his/her phone number. Then create a RelativeLayout, containing Image, Name, Phone Number, and a Button. Repeating creating that RelativeLayout  as many as your friend.

</br>

### Third: Nest the Grid inside ScrollView

</br>

### Fourth: Include the GridLayout Layout to activity_main.xml
Using `<include layout="@layout/grid_layout_name"/>`

### Fourth: Create an Integer Resources file and Embed to Layout File
Under *res/values/integers.xml* and *res/values-land/integers.xml* create a new integer number with different number. `<integer name="column_count">3</integer>`

</br>

### Fifth: Configure the GridLayout Using the integer.xml File
Add this properties inside GridLayout `app:columnCount="@integer/column_count">`

</br>

### Sixth: Run on Your Device


# Congrats!
