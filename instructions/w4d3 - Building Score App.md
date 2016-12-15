
![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Projects Week 4 - Build a Score App

### 1. Create New Project
Create new project in Android Studio:
- Application name  : Score App
- Domain name       : makersinstitute."yourName".com
- Package name      : com."yourName".makersinstitute.scoreapp
- Minimum sdk level : API 15, Android 4.0.3 (Ice Cream Sandwich)
- Use Empty Activity template
- Activity name     : MainActivity
- Layout Name       : activity_main

### 2. Build a Layout
1. Create layout like this, determine either LinearLayout or RelativeLayout is used. Try using `android:layout_weight` and combining both ViewGroup properly.

    ![main layout](../images/w4d3%20-%201.png)
2. Add attribute `android:id` to the score TextView which display the number of score on both team. For example,

    ```
    android:id="@+id/score_team_a"
    ```