![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 10 - Day 1 (SharedPreferences)

### <a name="lab11"></a>1. Create New Project

Create new project in android studio

- Application name : SharedPreference Example
- Minimum sdk level : API 15 Android 4.0.3 Ice Cream Sandwich
- Use Empty Activity template
- Activity name : MainActivity
- Layout Name : activity_main

---

### <a name="lab12"></a>2. Add SharedPreferences Object

1. Add Constant Variable to MainActivity
    ```
    //Key For Shared Preferences
    private static final String MYPREFERENCES = "myPreferences";

    //Key for data
    private static final String COUNTOPEN = "countOpen";
    ```

2. Add GLobal Variable to MainActivity 
    ```
    //Shared Preferences
    SharedPreferences mSharedPreferences;
    //to calculate how many applications open
    int countOpen;
    ```

3. Create SharedPreferences object in OnCreate method
    ```
     //get Shared Preferences
     mSharedPreferences = getSharedPreferences(MYPREFERENCES,MODE_PRIVATE);
    ```


4. Get data to calculate how many applications open from shared preferences 
    ```
    //get data count open
    countOpen = mSharedPreferences.getInt(COUNTOPEN,0);
    ```

5. Show toast that show how many applications open in OnCreate method
   ```
   //show toast
    if(countOpen == 0){
            Toast.makeText(MainActivity.this,"Aplikasi baru pertama kali dibuka",Toast.LENGTH_LONG).show();
        }else{
            Toast.makeText(MainActivity.this,"Aplikasi sudah " +  (countOpen + 1) +" kali dibuka",Toast.LENGTH_LONG).show();
        }
    }
   ``` 

6. Use Editor to save data how many applications open in onStop() method
    ```
   @Override
    protected void onStop() {
        super.onStop();
        //get Editor Object to make preferences changes
        SharedPreferences.Editor editor = mSharedPreferences.edit();
        editor.putInt(COUNTOPEN,(countOpen + 1));

        //Commit the edites
        editor.commit();
    }
    ```








