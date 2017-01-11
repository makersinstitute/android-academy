![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 8 - Day 1 (Earthquake App part 1)

### <a name="lab11"></a>1. Import Existing Project

- Go to this [GitHub project repository link](https://github.com/udacity/ud843-QuakeReport)
- Click on the “Download zip” button to download the app code.
- Unzip the downloaded file on your computer so that you have a “Quake Report” folder.
- Open Android Studio.
- Choose File > Import Project and select the “Quake Report” folder. 
- It may take some time for the project to be imported. If you have any issues, check the Troubleshooting document.
Once that app has successfully imported, run the app on your Android device (phone, tablet, or emulator). It should look like this screenshot.

   <img src="../images/w8d1%20-%201.png" width="250">

---

### <a name="lab12"></a>2. Show More Info On Each Earthquake

 <img src="../images/w8d1%20-%202.png" width="250">

1. Create XML layout for single list item that contains three TextView. 

   - Create new layout file called earthquake_list_item.xml
   ```
    <?xml version="1.0" encoding="utf-8"?>
    <!-- Layout for a single list item that displays an earthquake -->
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="16dp"
        xmlns:tools="http://schemas.android.com/tools"
        >

        <!-- TextView For Magnitude -->
        <TextView
            android:id="@+id/magnitude"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            tools:text="8.9"
            />

        <!-- TextView For Location -->
        <TextView
            android:id="@+id/location"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            tools:text="San Francisco, CA"
            />

        <!-- TextView For time -->
        <TextView
            android:id="@+id/date"
            android:layout_width="0dp"
            android:layout_weight="1"
            android:layout_height="wrap_content"
            tools:text="Mar 5, 2013"
            />
    </LinearLayout>    
   ```

2. Create a new earthquake class that represents a single eartquake where defined three global variable of type string(magnitude, location, and date) 
    ```
    public class Earthquake {

    //magnitude
    private String mMagnitude;

    //location
    private String mLocation;

    //date
    private String mDate;

    public Earthquake(String magnitude, String location, String date) {
        mMagnitude = magnitude;
        mLocation = location;
        mDate = date;
    }

    public String getMagnitude() {
        return mMagnitude;
    }

    public void setMagnitude(String magnitude) {
        mMagnitude = magnitude;
    }

    public String getLocation() {
        return mLocation;
    }

    public void setLocation(String location) {
        mLocation = location;
    }

    public String getDate() {
        return mDate;
    }

    public void setDate(String date) {
        mDate = date;
    }
    ```
3. Create Adapter called EarthQuakeAdapter.java 
    ```
    public class EarthQuakeAdapter extends ArrayAdapter<Earthquake> {

        public EarthQuakeAdapter(Context context, List<Earthquake> earthquakes){
            super(context,0,earthquakes);
        }

        @NonNull
        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            // Check if there is an existing list item view (called convertView) that we can reuse,
            // otherwise, if convertView is null, then inflate a new list item layout.
            View listItemView = convertView;
            if (listItemView == null) {
                listItemView = LayoutInflater.from(getContext()).inflate(
                        R.layout.earthquake_list_item, parent, false);
            }

            // Find the earthquake at the given position in the list of earthquakes
            Earthquake currentEarthquake = getItem(position);

            // Find the TextView with view ID magnitude
            TextView magnitudeView = (TextView) listItemView.findViewById(R.id.magnitude);
            magnitudeView.setText(currentEarthquake.getMagnitude());

            // Find the TextView with view ID location
            TextView locationView = (TextView) listItemView.findViewById(R.id.location);
            locationView.setText(currentEarthquake.getLocation());

            // Find the TextView with view ID date
            TextView dateView = (TextView) listItemView.findViewById(R.id.date);
            dateView.setText(currentEarthquake.getDate());

            // Return the list item view that is now showing the appropriate data
            return listItemView;
        }
    }
    ```
4. Create fake data of eartquake and set adapter to ListView in  EartQuakeActivity
    ```
    //Create a fake list of earthquake locations.
    ArrayList<Earthquake> earthquakes = new ArrayList<>();
    earthquakes.add(new Earthquake("7,2","San Francisco","Feb 2, 2014"));
    earthquakes.add(new Earthquake("6,2","London","Feb 2, 2014"));
    earthquakes.add(new Earthquake("5,2","Tokyo","Feb 2, 2014"));
    earthquakes.add(new Earthquake("4,2","Bandung","Feb 2, 2014"));
    earthquakes.add(new Earthquake("3,2","Garut","Feb 2, 2014"));
    earthquakes.add(new Earthquake("2,2","Paris","Feb 2, 2014"));
    earthquakes.add(new Earthquake("1,2","Moscow","Feb 2, 2014"));

    // Find a reference to the {@link ListView} in the layout
    ListView earthquakeListView = (ListView) findViewById(R.id.list);

    // Create a new {@link ArrayAdapter} of earthquakes
    EarthQuakeAdapter adapter = new EarthQuakeAdapter(this,earthquakes);

    // Set the adapter on the {@link ListView}
    // so the list can be populated in the user interface
    earthquakeListView.setAdapter(adapter);
    ```
---

### <a name="lab13"></a>3. Parse JSON Response In Quake Report App

The response we'll be using: http://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&starttime=2016-01-01&endtime=2016-01-31&minmag=6&limit=10

1. Copy/paste the code from the [QueryUtils.java](https://gist.github.com/udacityandroid/5cb10300bb10becb5f8185012b913c8e) class into a new file within your app
2. In the Quake Report app, write code to convert this JSON response and turn it into an ArrayList of Earthquake objects with the proper attributes.
Here’s some pseudocode of what needs to be done:
    ```
    Convert SAMPLE_JSON_RESPONSE String into a JSONObject
    Extract “features” JSONArray
    Loop through each feature in the array
    Get earthquake JSONObject at position i
    Get “properties” JSONObject
    Extract “mag” for magnitude
    Extract “place” for location
    Extract “time” for time
    Create Earthquake java object from magnitude, location, and time
    Add earthquake to list of earthquakes
    ```

    Here is code
    ```
    try {
            // TODO: Parse the response given by the SAMPLE_JSON_RESPONSE string and
            // build up a list of Earthquake objects with the corresponding data.
            JSONObject root = new JSONObject(SAMPLE_JSON_RESPONSE);
            JSONArray features = root.getJSONArray("features");
            for (int i = 0; i < features.length(); i++){
                JSONObject earthquake = features.getJSONObject(i);
                JSONObject properties = earthquake.getJSONObject("properties");
                String mag = properties.getString("mag");
                String place = properties.getString("place");
                String time = properties.getString("time");
                earthquakes.add(new Earthquake(mag,place,time));
            }
        }catch (JSONException e) {
            // If an error is thrown when executing any of the above statements in the "try" block,
            // catch the exception here, so the app doesn't crash. Print a log message
            // with the message from the exception.
            Log.e("QueryUtils", "Problem parsing the earthquake JSON results", e);
        }
    ```

3. Replace code of creating a fake list of eartquake to 
    ```
    // Create a fake list of earthquakes.
    ArrayList<Earthquake> earthquakes = QueryUtils.extractEarthquakes();
    ```

---

### <a name="lab14"></a>4. Display Date and Time Of Earthquake

 <img src="../images/w8d1%20-%203.png" width="250">

1. Add color item to color.xml

    ```
    <!-- Text color for the details of the earthquake in the list item -->
    <color name="textColorEarthquakeDetails">#B4BAC0</color>

    <!-- Text color for the primary location of the earthquake in the list item -->
    <color name="textColorEarthquakeLocation">#2B3D4D</color>
    ```

2. Update list item layout to show date and time in 2 separate textviews
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <!-- Layout for a single list item that displays an earthquake -->
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="16dp"
        xmlns:tools="http://schemas.android.com/tools"
        >

        <!-- TextView For Magnitude -->
        <TextView
            android:id="@+id/magnitude"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            tools:text="8.9"
            />

        <!-- TextView For Location -->
        <TextView
            android:id="@+id/location"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            tools:text="San Francisco, CA"
            />

        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:layout_marginLeft="16dp"
            android:layout_marginStart="16dp"
            android:orientation="vertical">
            
             <!-- TextView For Date -->
            <TextView
                android:id="@+id/date"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="end"
                android:textColor="@color/textColorEarthquakeDetails"
                android:textSize="12sp"
                tools:text="Mar 6, 2010" />

             <!-- TextView For Time -->
            <TextView
                android:id="@+id/time"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="end"
                android:textColor="@color/textColorEarthquakeDetails"
                android:textSize="12sp"
                tools:text="3:00 PM" />
        </LinearLayout>

    </LinearLayout>
    ```
2. In Earthquake class, delete variable date and store time as long 
    ```
    public class Earthquake {

        //magnitude
        private String mMagnitude;

        //location
        private String mLocation;

        //date
        private long mTimeMilliseconds;

        public Earthquake(String magnitude, String location, long timeMilliseconds) {
            mMagnitude = magnitude;
            mLocation = location;
            mTimeMilliseconds = timeMilliseconds;
        }

        public String getMagnitude() {
            return mMagnitude;
        }

        public void setMagnitude(String magnitude) {
            mMagnitude = magnitude;
        }

        public String getLocation() {
            return mLocation;
        }

        public void setLocation(String location) {
            mLocation = location;
        }

        public long getTimeMilliseconds() {
            return mTimeMilliseconds;
        }

        public void setTimeMilliseconds(long timeMilliseconds) {
            mTimeMilliseconds = timeMilliseconds;
        }
    }
    ```
3. In QueryUtils class, extract time as long when parsing the JSON response
    ```
    long time = properties.getLong("time");
    ```
4. Create a Date object, and Format date and time into a readable string for the user, in EarthquakeAdapter class
    ```
    public View getView(int position, View convertView, ViewGroup parent) {
        
        ...

        // Create a new Date object from the time in milliseconds of the earthquake
        Date dateObject = new Date(currentEarthquake.getTimeMilliseconds());

        // Find the TextView with view ID date
        TextView dateView = (TextView) listItemView.findViewById(R.id.date);
        // Format the date string (i.e. "Mar 3, 1984")
        String formattedDate = formatDate(dateObject);
        // Display the date of the current earthquake in that TextView
        dateView.setText(formattedDate);

        // Find the TextView with view ID time
        TextView timeView = (TextView) listItemView.findViewById(R.id.time);
        // Format the time string (i.e. "4:30PM")
        String formattedTime = formatTime(dateObject);
        // Display the time of the current earthquake in that TextView
        timeView.setText(formattedTime);


        // Return the list item view that is now showing the appropriate data
        return listItemView;
    }

    /**
     * Return the formatted date string (i.e. "Mar 3, 1984") from a Date object.
     */
    private String formatDate(Date dateObject) {
        SimpleDateFormat dateFormat = new SimpleDateFormat("LLL dd, yyyy");
        return dateFormat.format(dateObject);
    }

    /**
     * Return the formatted date string (i.e. "4:30 PM") from a Date object.
     */
    private String formatTime(Date dateObject) {
        SimpleDateFormat timeFormat = new SimpleDateFormat("h:mm a");
        return timeFormat.format(dateObject);
    }
    ```
---

### <a name="lab15"></a>5. Split Location Into Two Textviews

 <img src="../images/w8d1%20-%204.png" width="250">

Split location into a location offset (“74km NW of “) and a primary location (“Rumoi, Japan”) and display the 2 Strings in 2 separate TextViews.

1. Add String item to string.xml
    ```
    <!-- Default text to show with the earthquake location ("Near the Pacific-Antarctic Ridge")
         if no specific kilometer distance from the primary location is given [CHAR LIMIT=30] -->
    <string name="near_the">Near the</string>
    ```
1. Update list item layout 
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <!-- Layout for a single list item that displays an earthquake -->
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:padding="16dp"
        xmlns:tools="http://schemas.android.com/tools"
        >

        <!-- TextView For Magnitude -->
        <TextView
            android:id="@+id/magnitude"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:layout_gravity="center_vertical"
            tools:text="8.9"
            />

        <!-- TextView For Location -->
        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:layout_marginLeft="16dp"
            android:layout_marginStart="16dp"
            android:layout_weight="1"
            android:orientation="vertical">

            <TextView
                android:id="@+id/location_offset"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:ellipsize="end"
                android:fontFamily="sans-serif-medium"
                android:maxLines="1"
                android:textAllCaps="true"
                android:textColor="@color/textColorEarthquakeDetails"
                android:textSize="12sp"
                tools:text="30km S of" />

            <TextView
                android:id="@+id/primary_location"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:ellipsize="end"
                android:maxLines="2"
                android:textColor="@color/textColorEarthquakeLocation"
                android:textSize="16sp"
                tools:text="Long placeholder location that should wrap to more than 2 lines of text" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_vertical"
            android:layout_marginLeft="16dp"
            android:layout_marginStart="16dp"
            android:orientation="vertical">

            <TextView
                android:id="@+id/date"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="end"
                android:textColor="@color/textColorEarthquakeDetails"
                android:textSize="12sp"
                tools:text="Mar 6, 2010" />

            <TextView
                android:id="@+id/time"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="end"
                android:textColor="@color/textColorEarthquakeDetails"
                android:textSize="12sp"
                tools:text="3:00 PM" />
        </LinearLayout>

    </LinearLayout>
    ```

2. Split location into 2 tetview in EarthquakeAdapter. 
    ```
    public class EarthQuakeAdapter extends ArrayAdapter<Earthquake> {

        /**
            * The part of the location string from the USGS service that we use to determine
            * whether or not there is a location offset present ("5km N of Cairo, Egypt").
            */
        private static final String LOCATION_SEPARATOR = " of ";


        @NonNull
        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            
            ...

            // Get the original location string from the Earthquake object,
            // which can be in the format of "5km N of Cairo, Egypt" or "Pacific-Antarctic Ridge".
            String originalLocation = currentEarthquake.getLocation();

            // If the original location string (i.e. "5km N of Cairo, Egypt") contains
            // a primary location (Cairo, Egypt) and a location offset (5km N of that city)
            // then store the primary location separately from the location offset in 2 Strings,
            // so they can be displayed in 2 TextViews.
            String primaryLocation;
            String locationOffset;

            // Check whether the originalLocation string contains the " of " text
            if (originalLocation.contains(LOCATION_SEPARATOR)) {
                // Split the string into different parts (as an array of Strings)
                // based on the " of " text. We expect an array of 2 Strings, where
                // the first String will be "5km N" and the second String will be "Cairo, Egypt".
                String[] parts = originalLocation.split(LOCATION_SEPARATOR);
                // Location offset should be "5km N " + " of " --> "5km N of"
                locationOffset = parts[0] + LOCATION_SEPARATOR;
                // Primary location should be "Cairo, Egypt"
                primaryLocation = parts[1];
            } else {
                // Otherwise, there is no " of " text in the originalLocation string.
                // Hence, set the default location offset to say "Near the".
                locationOffset = getContext().getString(R.string.near_the);
                // The primary location will be the full location string "Pacific-Antarctic Ridge".
                primaryLocation = originalLocation;
            }

            // Find the TextView with view ID location
            TextView primaryLocationView = (TextView) listItemView.findViewById(R.id.primary_location);
            // Display the location of the current earthquake in that TextView
            primaryLocationView.setText(primaryLocation);

            // Find the TextView with view ID location offset
            TextView locationOffsetView = (TextView) listItemView.findViewById(R.id.location_offset);
            // Display the location offset of the current earthquake in that TextView
            locationOffsetView.setText(locationOffset);
            
            ...
        }
    }
    ```
---

### <a name="lab16"></a>6. Magnitude As Decimal Value

1. In the Earthquake class , store magnitude as a double 
    ```
    //magnitude
    private double mMagnitude;
    ```
2. In the QueryUtils class, extract magnitude as double when parsing the JSON response
    ```
    double mag = properties.getDouble("mag");
    ```
3. In the EarthquakeAdapter class, use decimalFormat so that one decimal place is always shown
    ```
    /**
     * Return the formatted magnitude string showing 1 decimal place (i.e. "3.2")
     * from a decimal magnitude value.
     */
    private String formatMagnitude(double magnitude) {
        DecimalFormat magnitudeFormat = new DecimalFormat("0.0");
        return magnitudeFormat.format(magnitude);
    }
    ```

    ```
    // Find the TextView with view ID magnitude
    TextView magnitudeView = (TextView) listItemView.findViewById(R.id.magnitude);

    // Format the magnitude to show 1 decimal place
    String formattedMagnitude = formatMagnitude(currentEarthquake.getMagnitude());
    // Display the magnitude of the current earthquake in that TextView
    magnitudeView.setText(formattedMagnitude);
    ```

### <a name="lab16"></a>7. Circle Background for Magnitude

1. Add Color resource 

    ```
     <!-- Color for an earthquake with magnitude 0 and 2 -->
    <color name="magnitude1">#4A7BA7</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 2 and 3 -->
    <color name="magnitude2">#04B4B3</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 3 and 4 -->
    <color name="magnitude3">#10CAC9</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 4 and 5 -->
    <color name="magnitude4">#F5A623</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 5 and 6 -->
    <color name="magnitude5">#FF7D50</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 6 and 7 -->
    <color name="magnitude6">#FC6644</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 7 and 8 -->
    <color name="magnitude7">#E75F40</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 8 and 9 -->
    <color name="magnitude8">#E13A20</color>

    <!-- Magnitude circle color for an earthquake with magnitude between 9 and 10 -->
    <color name="magnitude9">#D93218</color>

    <!-- Magnitude circle color for an earthquake with magnitude over 10 -->
    <color name="magnitude10plus">#C03823</color>
    ```
2. Define a new drawable for the colored circle. Within the project directory pane of Android Studio, right click on the res/drawable folder to add a new drawable resource XML file. Name the file “magnitude_circle”. Replace the contents of the res/drawable/magnitude_circle.xml file with the below XML. 

    In magnitude_circle.xml:
    
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <!-- Background circle for the magnitude value -->
    <shape xmlns:android="http://schemas.android.com/apk/res/android"
        android:shape="oval">
        <solid android:color="@color/magnitude1" />
        <size
            android:width="36dp"
            android:height="36dp" />
        <corners android:radius="18dp" />
    </shape> 
     ```

3. Modify the list item layout so that the background attribute of the magnitude TextView refers to the new drawable resource we just defined (android:background="@drawable/magnitude_circle")
    ```
    <TextView
        android:id="@+id/magnitude"
        android:layout_width="36dp"
        android:layout_height="36dp"
        android:layout_gravity="center_vertical"
        android:background="@drawable/magnitude_circle"
        android:fontFamily="sans-serif-medium"
        android:gravity="center"
        android:textColor="@android:color/white"
        android:textSize="16sp"
        tools:text="8.9" />
    ```

4. Modify the EarthquakeAdapter so that the correct color gets set on the background circle for each list item

    In EarthquakeAdapter getView():

    ```
    // Set the proper background color on the magnitude circle.
    // Fetch the background from the TextView, which is a GradientDrawable.
    GradientDrawable magnitudeCircle = (GradientDrawable) magnitudeView.getBackground();

    // Get the appropriate background color based on the current earthquake magnitude
    int magnitudeColor = getMagnitudeColor(currentEarthquake.getMagnitude());

    // Set the color on the magnitude circle
    magnitudeCircle.setColor(magnitudeColor);
    ```

5. In the EarthquakeAdapter, define new helper adapter method. Use switch statement to find the appropriate color resource based on the magnitude value

    In EarthquakeAdapter.java:

    ```
    import android.support.v4.content.ContextCompat;

    … 

    private int getMagnitudeColor(double magnitude) {
        int magnitudeColorResourceId;
        //finding the closest integer less than the decimal value
        int magnitudeFloor = (int) Math.floor(magnitude);
        
        switch (magnitudeFloor) {
            case 0:
            case 1:
                magnitudeColorResourceId = R.color.magnitude1;
                break;
            case 2:
                magnitudeColorResourceId = R.color.magnitude2;
                break;
            case 3:
                magnitudeColorResourceId = R.color.magnitude3;
                break;
            case 4:
                magnitudeColorResourceId = R.color.magnitude4;
                break;
            case 5:
                magnitudeColorResourceId = R.color.magnitude5;
                break;
            case 6:
                magnitudeColorResourceId = R.color.magnitude6;
                break;
            case 7:
                magnitudeColorResourceId = R.color.magnitude7;
                break;
            case 8:
                magnitudeColorResourceId = R.color.magnitude8;
                break;
            case 9:
                magnitudeColorResourceId = R.color.magnitude9;
                break;
            default:
                magnitudeColorResourceId = R.color.magnitude10plus;
                break;
        }
        return ContextCompat.getColor(getContext(), magnitudeColorResourceId);
    }
    ```








