![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 4 - Day 3 (Milk Ordering App Part 2)

### <a name="lab11"></a>1. Build Layout Part 4
- Change the position of the increment button, quantitymilkTextView, and decrement button.
- Use Horizontal LinearLayout
 
  ![Gambar 1](../images/w5d1%20-%204.png)

---

### <a name="lab12"></a>2. Error Handling
- Make that number of order milk is not less than 0 when clicking the decrement button

### <a name="lab13"></a>3. Show total price of order milk
1. Add a global variable to MainActivity.java with an integer data type with the name of totalPriceOfOrderMilk to store total price of order milk with default value is 0.   
    ```
    int totalPriceOfOrderMilk = 0;  
    ```

2. Add a global constant variable to MainActivity.java with an integer data type with the name of priceOfMilk to store price per milk with value is 5.   
    ```
   final int priceOfMilk = 5; 
    ``` 

3. Create new global variabel TextView with name totalPriceTextView to MainActivity.java
    ```
    TextView totalPriceTextView; 
    ``` 

4. Assign value to quantityTextView
    ```
    totalPriceTextView = (TextView)findViewById(R.id.priceOrderMilkTextView);
    ```

5. Add atribut `android:onClick` to button that show total price of milk order in activity_main.xml
    ```
    android:onClick="showTotalPrice"
    ```

6. Add void function showTotalPrice() to show total of milk order
    ```
    public void showTotalPrice(View view){
        //Calculate total price of order milk
        totalPriceOfOrderMilk = priceOfMilk * quantityOfMilk;

        //show total price 
        //add your code ...
    }
    ```
7. Run App your device

---
