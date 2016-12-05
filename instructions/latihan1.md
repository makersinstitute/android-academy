![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 5 - Day 1 (Milk Ordering App Part 1)

### <a name="lab11"></a>1. Create New Project
Create new project in android studio
- Application name  : Milk Ordering
- Domain name       : makersinstitute."your name".com
- Package name      : com."yourname".makersinstitute.milkordering
- Minimum sdk level : API 15 Android 4.0.3 Ice Cream Sandwich
- Use Empty Activity template
- Activity name     : MainActivity
- Layout Name       : activity_main

---

### <a name="lab12"></a>2. Build Layout Part 1
1. Create layout like picture 1 
2. Add atribut `android:id` to TextView which display the number of milk order with value id `@+id/quantityMilkTextView`
```
android:id="@+id/quantityMilkTextView"
```

---

### <a name="lab13"></a>3. Build Layout Part 2
1. Add 2 text view to layout. 
2. Add atribut `android:id` to TextView which displays the total price of milk order with value id `@+id/priceOrderMilkTextView`. 
```
android:id="@+id/priceOrderMilkTextView"
```

### <a name="lab14"></a>4. Build Layout Part 3
1. Add 2 button for increment and decrement the number of milk order

---

### <a name="lab15"></a>5. Increment Quantity of Order and Displaying Quantity of Milk Ordering 
1. Add a `global` variable to `MainActivity.java` with an integer data type with the name of `quantityOfMilk` to store the the number of milk order with default value is 0. 
```
int quantityOfMilk = 0;
```
2. Add atribut `android:onClick` to button that increment the number of milk order in `activity_main.xml` 
```
android:onClick="incrementOrder"
```
3. Create new global variabel `TextView` with name `quantityTextView` to MainActivity.java
```
TextView quantityTextView;
```
4. Assign value to `quantityTextView` use `findViewById()` in `onCreate()` function 
```
quantityTextView = (TextView)findViewById(R.id.quantityMilkTextView);
``` 
5. Add void function `incrementOrder()` to increment the number of milk order and displaying an quantity of ordering milk 
```Java
public void incrementOrder(View view){
    //increment quantity of order milk
    quantityOfMilk += 1;
    //displaying a quantity of order milk
    quantityTextView.setText("" + quantityOfMilk);
}
``` 
6. Run app on your android device 

---

### <a name="lab15"></a>6. Decrement Quantity of Order  
1. Add atribut `android:onClick` to button that decrement the number of orders milk in `activity_main.xml` 
```
android:onClick="decrementOrder"
``` 
2. Add void function `decrementOrder()` to decrement the number of milk order 
```Java
public void decrementOrder(View view){
    //decrement quantity of order milk
    quantityOfMilk -= 1;
    //displaying a quantity of order milk
    quantityTextView.setText("" + quantityOfMilk);
}
``` 
3. Run app on your android device 

---















