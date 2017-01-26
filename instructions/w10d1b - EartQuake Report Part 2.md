![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 10 - Day 1 (EarthQuake Report Part#3)

### <a name="lab11"></a>1.Import Project


### <a name="lab12"></a>2. Add Uses Permission and String item 

1. Add user permission to access network state in android manifest
    ```
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    ```

2. Add String item to string.xml
    ```
    <!-- text to display in list when there are no eartquake -->
    <string name="no_eartquake">No eartquake found.</string>

    <!-- Error Message when there is no internet connection -->
    <string name="no_internet_connection">No Internet Connection</string>

    <!-- Seetting menu item -->
    <string name="setting_menu_item">Setting</string>

    <!-- Setting activity title -->
    <string name="setting_title">Setting</string>

    <!-- Text for refresh -->
    <string name="refresh">Refresh</string>

    <!-- Setting for minimum magnitude Preference -->
    <string name="setting_min_magnitude_label">Minimum Magnitude</string>
    <string name="setting_min_magnitude_key" translatable="false">min_magnitude</string>
    <string name="setting_min_magnitude_default" translatable="false">6</string>

    ```

### <a name="lab13"></a>3. Add Preference To EartquakeReport Application

1. Create android resource file with name prefs.xml in res/xml folder. 
    Folder res -> Right click -> New -> Android Resource File 

2.  Add content to prefs.xml
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <PreferenceScreen
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:title="@string/setting_title"
        >

        <!-- Preferences for Miniml Magnitude -->
        <EditTextPreference
            android:defaultValue="@string/setting_min_magnitude_default"
            android:inputType="numberDecimal"
            android:key="@string/setting_min_magnitude_key"
            android:selectAllOnFocus="true"
            android:title="@string/setting_title"
            />

    </PreferenceScreen>
    ```

3. Create new class with name PrefsActivity  with the following content
    ```
    public class PrefsActivity extends PreferenceActivity
            implements Preference.OnPreferenceChangeListener {

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            //set content to file prefs.xml in folder xml
            addPreferencesFromResource(R.xml.prefs);

            //Given the key of a preference, we can use PreferenceFragment's findPreference() method to get the Preference object
            Preference minMagnitude = findPreference(getString(R.string.setting_min_magnitude_key));
            bindPreferenceSummaryToValue(minMagnitude);
        }

        //helper method to read the current value of the preference stored in the SharedPreferences on the device,
        // and display that in the preference summary (Setting Screen)
        private void bindPreferenceSummaryToValue(Preference preference) {
            preference.setOnPreferenceChangeListener(this);
            SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(preference.getContext());
            String preferenceString = sharedPreferences.getString(preference.getKey(), "");
            onPreferenceChange(preference,preferenceString);
        }

        //OnPreferenceChangeListener interface to get notified when a preference changes
        @Override
        public boolean onPreferenceChange(Preference preference, Object value) {
            String preferenceValue = value.toString();
            preference.setSummary(preferenceValue);
            return true;
        }
    }
    ```

4. Add prefsActivity to AndroidManifest  
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
            package="com.hidayatasep.makersinstitute.earthquakereport">

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:supportsRtl="true"
            android:theme="@style/AppTheme">
            <activity android:name=".MainActivity">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN"/>

                    <category android:name="android.intent.category.LAUNCHER"/>
                </intent-filter>
            </activity>

            <activity
                android:name=".PrefsActivity"
                android:label="@string/setting_title">
            </activity>

        </application>

    </manifest>
    ```
   
### <a name="lab13"></a>3. Create menu for access preference 
1. Create new menu file with name menu_main.xml in res/menu folder  with the following content : 
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <menu
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        tools:context=".MainActivity"
        >
        <item
            android:title="@string/setting_menu_item"
            android:id="@+id/action_setting"
            android:orderInCategory="1"
            app:showAsAction="never"/>
        <item
            android:title="@string/refresh"
            android:id="@+id/action_refresh"
            android:orderInCategory="2"
            app:showAsAction="never"/>
    </menu>
    ```

2. Add onCreateOptionMenu method and onOptionItemSelected to MainActivity
    ```
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main,menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();
        if(id == R.id.action_setting){
            Intent intent= new Intent(this,PrefsActivity.class);
            startActivity(intent);
            return true;
        }else if(id == R.id.action_refresh){
           
        }
        return super.onOptionsItemSelected(item);
    }
    ```
### <a name="lab13"></a>4. Update layout activity_main
1. Add textview for show information if no eartquake data to activity_main.xml 
    ```
    <!-- Empty view is only visible when the list has no items -->
   <TextView
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_centerInParent="true"
       android:textAppearance="?android:textAppearanceMedium"
       android:id="@+id/empty_view"
       android:text="@string/no_eartquake"
       android:visibility="invisible"
       />
    ```

2. Add textview and button for show information if no internet connetion to activity_main.xml
    ```
    <!-- No Connection view is only visible when No internet connection -->
   <TextView
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_centerInParent="true"
       android:textAppearance="?android:textAppearanceMedium"
       android:id="@+id/noInternetConnectionLabel"
       android:text="@string/no_internet_connection"
       android:visibility="invisible"

       />

   <!-- No Connection button is only visible when No internet connection -->
   <Button
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:id="@+id/refreshButton"
       android:background="@color/magnitude5"
       android:textColor="@android:color/white"
       android:layout_below="@+id/noInternetConnectionLabel"
       android:text="@string/refresh"
       android:layout_marginTop="10dp"
       android:layout_centerHorizontal="true"
       android:visibility="invisible"
       />
    ```
3. Add ProggressBar to activity_main.xml to show progress 
    ```
    <!-- Loading Indocatior for user feedback between queries to usgs -->
   <ProgressBar
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:id="@+id/loadingIndicator"
       style="@style/Widget.AppCompat.ProgressBar"
       android:visibility="invisible"
       android:layout_centerInParent="true"
       />

    ```

### <a name="lab13"></a>5. Update MainActivity
1. Add global variabel to store TextView, Button, and ProggressBar
    ```
    //view for no data
    TextView emptyEartquake;

    //view for no connecton
    TextView noInternetConnectionLabel;
    Button refreshButton;

    //loading view
    ProgressBar loadingProgress;
    ```

2. Assign view to variabel in onCreate() method
    ```
    emptyEartquake = (TextView) findViewById(R.id.empty_view);

        refreshButton = (Button) findViewById(R.id.refreshButton);
        refreshButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                refreshData();
            }
        });
        noInternetConnectionLabel = (TextView) findViewById(R.id.noInternetConnectionLabel);
        loadingProgress = (ProgressBar) findViewById(R.id.loadingIndicator);
    ```

3. Add showLoadingIndicator() method for show ProgressBar in MainActivity
    ```
    //show progress bar
    public void showLoadingIndicator(){
        if(loadingProgress.getVisibility() == View.INVISIBLE){
            //show progress bar 
            //and hide another view 
            loadingProgress.setVisibility(View.VISIBLE);
            earthquakeListView.setVisibility(View.INVISIBLE);
            emptyEartquake.setVisibility(View.INVISIBLE);
            noInternetConnectionLabel.setVisibility(View.INVISIBLE);
            refreshButton.setVisibility(View.INVISIBLE);
            refreshButton.setEnabled(false);
        }
    }
    ```
3. Add showList() method for show eartquake list and hide progress bar in MainActivity
    ```
    //method to show eartquake list and hide progress bar
    public void showList(){
        loadingProgress.setVisibility(View.INVISIBLE);
        earthquakeListView.setVisibility(View.VISIBLE);
    }
    ```

4. Add showEmptyData() method for show eartquake empty textview and hide progress bar
    ```
    //method to show eartquake empty textview and hide progress bar
    public void showEmptyData(){
        loadingProgress.setVisibility(View.INVISIBLE);
        emptyEartquake.setVisibility(View.VISIBLE);
    }
    ```

5. Add showNoInternetView() method for show no internet connection view  and hide progress bar
    ```
     //method to show no internet connection vie  and hide progress bar
    public void showNoInternetView(){
        loadingProgress.setVisibility(View.INVISIBLE);
        noInternetConnectionLabel.setVisibility(View.VISIBLE);
        refreshButton.setVisibility(View.VISIBLE);
        refreshButton.setEnabled(true);
    }
    ```
6. Add getLinkEarthQuake() method to get link to get data from usgs 
    ```
    //get link to get data from usgs
    //use minimal magnitude from preferences
    private String getLinkEartQuake() {
        //use min magnitude from preferences
        SharedPreferences sharedPrefences = PreferenceManager.getDefaultSharedPreferences(this);
        String minMagnitude = sharedPrefences.getString(
                getString(R.string.setting_min_magnitude_key),
                getString(R.string.setting_min_magnitude_default));
        //return url
        String url = "http://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&orderby=time&minmag=" + minMagnitude +"&limit=30";
        return url;
    }
    ```

7. update DownloadTaskEartQuake so as to have a string parameter for url to get data from usgs and add onPreexecute method that contains a function to delete data
    ```
     private class DownloadTaskEartQuake extends AsyncTask<String,Void,ArrayList<EarthQuake>>{

        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            //clear data in list
            adapter.clear();
        }

        @Override
        protected ArrayList<EarthQuake> doInBackground(String... urlString) {

            //set up URL
            URL url = QueryUtils.createURL(urlString[0]);
            ...
    ```

8. Add refreshData() method to refreh item in Eartquake list and get Data from server 
    ```
    //method to get data from server
    private void refreshData() {
        showLoadingIndicator();
        // Get a reference to the ConnectivityManager to check state of network connectivity
        ConnectivityManager connMgr = (ConnectivityManager)
                getSystemService(Context.CONNECTIVITY_SERVICE);

        // Get details on the currently active default data network
        NetworkInfo networkInfo = connMgr.getActiveNetworkInfo();
        // If there is a network connection, fetch data
        if (networkInfo != null && networkInfo.isConnected()) {
            String url = getLinkEartQuake();
            new DownloadTaskEartQuake().execute(url);
        }else{
            showNoInternetView();
        }
    }
    ```

9. Update onPostExecute method in DownloadTaskEartQuake 
    ```
    @Override
        protected void onPostExecute(ArrayList<EarthQuake> earthQuakes) {
            super.onPostExecute(earthQuakes);
            //add eartquake to list view
            if(earthQuakes.isEmpty()){
                //if no data show empty text view 
                showEmptyData();
            }else{
                showList();
                for(int i = 0;i < earthQuakes.size(); i++){
                    //add eartquake to list 
                    adapter.add(earthQuakes.get(i));
                }
            }
        }
    ```

9. Call refrehData in onResume() 
    ```
    //refresh data
    refreshData();
    ```
 
9. add setOnClickListener to refreshButton
    ```
    refreshButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                refreshData();
            }
        });
    ```

10. Call refrehData() method in onOptionItemSelected 
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();
        if(id == R.id.action_setting){
            Intent intent= new Intent(this,PrefsActivity.class);
            startActivity(intent);
            return true;
        }else if(id == R.id.action_refresh){
            refreshData();
        }
        return super.onOptionsItemSelected(item);
    }





