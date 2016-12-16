![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 5 - Day 2 Part 1 (Ordering App #2)


We will continue yesterday's Ordering App. Adding a checkbox to make it look like this:

![Final App](../images/w5d2%20-%202.png)


## Add the Chocolate Topping Checkbox Solution
#### Step 1 : Change the XML
First, let’s update the XML. We want to add a checkbox in the LinearLayout. That can be done with the code here.

Specifically, we add the following lines:
```
   <CheckBox
       android:id="@+id/chocolate_checkbox"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="24dp"
       android:text="Chocolate"
       android:textSize="16sp" />
```
</br>

#### Step 2 : The Plan for the Java Changes
Given the new checkbox, what should happen when the submit button is clicked? Let’s think about the final pseudocode for what will happen when the submit order button is clicked.

When the submit order button is clicked:

1. Set `hasWhippedCream` to the value of the checked state in the whipped cream CheckBox.
2. Set hasChocolate to the value of the checked state in the chocolate CheckBox.
3. Calculate the price and store this value in the price variable.
4. Create the order summary and store this String in the message variable.
5. Display message on the screen.
---

Creating the order summary
1. Set `priceMessage` to the name.
2. Append to priceMessage "Add whipped cream?" and the value of whether the coffee has whipped cream.
3. **Append to priceMessage "Add chocolate?" and the value of whether the coffee has chocolate.**
4. Append to priceMessage "Quantity:" and the quantity.
5. Append to priceMessage "Total: $" and the total price.
6. Append to priceMessage "Thank you!"
7. Return the priceMessage

The steps in bold are the ones that need to be implemented or fully implemented given that you’re adding this new checkbox.

</br>

#### Step 3 : Change the Java
**Set `hasChocolate` to the value of the chocolate CheckBox**

In submitOrder method, we need to get the checked state from the chocolate CheckBox. Here are the steps for doing that:

1. You don’t have a reference to that object, so you’ll need to get the CheckBox object using findViewById. You’ll need to pass the chocolate checkbox id.

 ```
 findViewById(R.id.chocolate_checkbox);
```

2. You can then store the returned value in a Checkbox object.
```
 CheckBox chocolateCheckBox = findViewById(R.id.chocolate_checkbox); 
```

3. Because findViewById returns a generic View, you’ll need to cast it to a CheckBox.
```
 CheckBox chocolateCheckBox = (CheckBox)findViewById(R.id.chocolate_checkbox); 
```

4. Now you’ll need to call a method on the object. In the documentation you’ll see there is a method called `isChecked`.

5. Save what is returned from isChecked in a boolean variable.
```
 boolean hasChocolate = chocolateCheckBox.isChecked(); 
```

The final code for getting the value of the checkbox and storing it in a variable is:
```
// Figure out if the user wants chocolate topping
CheckBox chocolateCheckBox = (CheckBox) findViewById(R.id.chocolate_checkbox);
boolean hasChocolate = chocolateCheckBox.isChecked();
```
</br>

**Creating the order summary**

The only issue here is that you need access to the boolean you just retrieved in the createOrderSummary method. We’ll need to pass the boolean to createOrderSummary method as an input parameter.

1. Here is the code when you call the createOrderSummary method that passes in the hasChocolate boolean:
```
createOrderSummary(price, hasWhippedCream, hasChocolate);
```

2. This will produce an error in Android Studio because the method call does not match the method definition (the number of parameters is different). Therefore, you also need to change where the method is defined so that it has this new parameter.
```
private String createOrderSummary(int price, boolean addWhippedCream, boolean addChocolate)
```

3. Update the javadoc comment as well to document this new input parameter addChocolate.
```
/**
 * Create summary of the order.
 *
 * @param addWhippedCream is whether or not the user wants whipped cream topping
 * @param addChocolate is whether or not the user wants whipped cream topping
 * @param price of the order
 * @return text summary
 */
 ```
 </br>
 
**Append to priceMessage "Add chocolate?" and the value of whether the coffee has chocolate.**

Okay, now you need to change the message that is displayed to show whether the customer ordered chocolate or not. The string to do that is below:
```
"Add chocolate?" + addChocolate 
```
The final code for create order summary that appends this string is:
```
private String createOrderSummary(int price, boolean addWhippedCream, boolean addChocolate) {
   String priceMessage = "Name: Lyla the Labyrinth";
   priceMessage += "\nAdd whipped cream? " + addWhippedCream;
   priceMessage += "\nAdd chocolate? " + addChocolate;
   priceMessage += "\nQuantity: " + quantity;
   priceMessage += "\nTotal: $" + price;
   priceMessage += "\nThank you!";
   return priceMessage;
}
```