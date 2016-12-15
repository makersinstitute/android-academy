![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 5 - Day 1 (Ordering App)

### 1. Continue Your Milk Ordering App

![Gambar 1](../images/w5d1%20-%204.png)
  

---

### 2. Using Resources
- Move hardcoded String inside XML or Java Code, into resources folder under *res/values/strings.xml*
- Also, if you define a margin size, font size, or else, put it also under *res/values/dimens.xml*

---

### 3. Change the *Price* text into *Order Summary*

![Gambar 1](../images/w5d1%20-%2011.png)

- Change the specific String under *res/values/strings.xml*

---

### 4. Define a Specific Method to Calculate the Total Milk Price which returns the totalPrice

```
public int calculateTotalPrice(int numberOfMilk, int milkPrice) {
	int totalPrice = numberOfMilk * milkPrice;
    return totalPrice;
}
```

- Change the specific String under *res/values/strings.xml*

---

### 5. Calling the Calculation Method

```
int totalPrice = calculateTotalPrice(numberOfMilk, milkPrice);
```

---

### 6. Order Summary Text

1. We see from previous image, the summary contains several lines of text.
2. To create them, we use concatenation in String

``` java
String orderSummary = "Name: Makers Guy";
orderSummary += "Ordering";
orderSummary += numberOfMilk;
orderSummary += "Glass of Milk";
orderSummary += "Each Milk is";
orderSummary += milkPrice;
orderSummary += "The total price is";
orderSummary += totalPrice;
orderSummary += "Thank you";
```
3. If the result is weird, try to use `\n` inside quotation mark, and see what happens.

---

### 7. Define a Specific Method to Display the Order Summary Text

``` java
public void displayOrderSummary() {
	
    // put the previous code snippet, the total price 
    // and the concatenation of String inside here
    // cast the data to the view
    
    TextView totalPriceTextView = (TextView) findViewById(R.id.total_price_text_view);
    totalPriceTextView.setText(orderSummary);

}
```

### 8. Display on Order Button Pressed

```
public void showTotalPrice(View view){

    displayOrderSummary();

}
```

### 9. Make sure you know where to put the code.
- Decide which one is *Global Variable* and *Local Variable*
- The first line of code called before the second line, so make sure you have a correct code sequence to prevent something unexpected.
- 



---