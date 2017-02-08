![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 11 - Day 1 (Google Maps)

### <a name="lab11"></a>1. Install The Google Play Services SDK

1. Open android sdk folder 

2. Open SDK Manager.exe

3. Check Google Play service 

---

### <a name="lab12"></a>2. Create a Google Maps Project

1. Start Android Studio

2. Create a new Project

3. Enter app name, company domain, and project location. Then click <b>Next</b>

4. Select the form factors you need for your app. If you're not sure what you need, just select Phone and Tablet. Then click <b>Next</b>.

5. Select <b>Google Maps Activity</b> in the 'Add an activity to Mobile' dialog. Then click <b>Next</b>.

6. Enter the activity name, layout name and title as prompted. The default values are fine. Then click <b>Finish</b>.

---

### <a name="lab13"></a>3. Get a Google Maps API key

Your application needs an API key to access the Google Maps servers. The type of key you need is an API key with restriction for Android apps. The key is free. You can use it with any of your applications that call the Google Maps Android API, and it supports an unlimited number of users.

1. Copy the link provided in the google_maps_api.xml file and paste it into your browser. The link takes you to the Google API Console and supplies the required information to the Google API Console via URL parameters, thus reducing the manual input required from you.

2. Follow the instructions to create a new project on the Google API Console or select an existing project.

3. Create an Android-restricted API key for your project. 

4. Copy the resulting API key, go back to Android Studio, and paste the API key into the string element in the <b>google_maps_api.xml</b> file.

---

### <a name="lab13"></a>4. Getting the Last Known Location

1.  Change gradle build on the code 
    ```
    compile 'com.google.android.gms:play-services:10.0.1'
    ```
    to 
    ```
    compile 'com.google.android.gms:play-services:8.4.0'
    ```

1.  Add App Permission in AndroidManifest.xml
    ```
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    ```

2. Add Statement for show Button for back to Location user 
    ```
    @Override
    public void onMapReady(GoogleMap googleMap) {
        mMap = googleMap;
        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    ActivityCompat#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for ActivityCompat#requestPermissions for more details.
            return;
        }
        mMap.setMyLocationEnabled(true);
    }
    ``` 

2.  Add global variable Google API Client inside MapsActivity 
    ```
    private GoogleApiClient mGoogleApiClient;
    ``` 

3. Initialize GoogleApiClient object inside onCreate() method
   ```
   //create google Api client
   mGoogleApiClient = new GoogleApiClient.Builder(this)
            .addConnectionCallbacks(this)
            .addOnConnectionFailedListener(this)
            .addApi(LocationServices.API)
            .build();
   ```

4. Implementing Location Callbacks to Maps Activity
    ```
    public class MapsActivity extends FragmentActivity implements OnMapReadyCallback, 
        GoogleApiClient.ConnectionCallbacks, 
        GoogleApiClient.OnConnectionFailedListener {

        ...
        
        @Override
        public void onConnected(@Nullable Bundle bundle) {
            Log.d(TAG, "Location service connected");
        }

        @Override
        public void onConnectionSuspended(int i) {
            Log.d(TAG,"Location service suspended");
        }


        @Override
        public void onConnectionFailed(@NonNull ConnectionResult connectionResult) {
            Log.d(TAG,"Location service failed");
        }

        ...

        }
    ```

5. Connecting and disconnecting GoogleApiClient
    ```
    @Override
    protected void onResume() {
        super.onResume();
        //connecting to google api client object
        mGoogleApiClient.connect();
        
    }

    @Override
    protected void onPause() {
        super.onPause();
        if(mGoogleApiClient.isConnected()){
            //disconnecting to google api client object
            mGoogleApiClient.disconnect();
        }
    }
    ```

6. Add method for Update position in Map 
    ```
    //method for update posistion user in Map
    private void handleNewLocation(Location location) {
        Log.d(TAG,location.toString());
        //clear marker
        mMap.clear();
        //latitude dan longitude
        double currentLatitude = location.getLatitude();
        double currentLongitude = location.getLongitude();
        LatLng latLng = new LatLng(currentLatitude,currentLongitude);

        //add marker to map
        MarkerOptions marker = new MarkerOptions()
                .position(latLng)
                .title("I am Here!");
        mMap.addMarker(marker);
        //set camera to marker
        mMap.moveCamera(CameraUpdateFactory.newLatLng(latLng));
    }
    ```

7. Add statement for get Location of user and show on Maps in onConnected() method  
    ```
    @Override
    public void onConnected(@Nullable Bundle bundle) {
        Log.d(TAG, "Location service connected");
        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    ActivityCompat#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for ActivityCompat#requestPermissions for more details.
            return;
        }
        //get Last Location
        Location location = LocationServices.FusedLocationApi.getLastLocation(mGoogleApiClient);
        if (location != null) {
            //update map
            handleNewLocation(location);
        }
    }
    ```

### <a name="lab15"></a>5. Receiving Location Update

1. Implementing LocationListener to MapsActivity
    ```
    public class MapsActivity extends FragmentActivity implements OnMapReadyCallback,
            GoogleApiClient.ConnectionCallbacks,
            GoogleApiClient.OnConnectionFailedListener, 
            LocationListener{

            ...

                @Override
                public void onLocationChanged(Location location) {
                    handleNewLocation(location);
                }

            }
            
    ```
2. Add Global variable LocationRequest inside MapsActivity
    ```
    private LocationRequest mLocationRequest;
    ```

3. iniate LocationRequest object inside onCreate() method. 
    ```
    //inisiate LocationRequest object 
    mLocationRequest = LocationRequest.create()
                .setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY)
                .setInterval(10 * 1000)
                .setFastestInterval(1 * 1000);
    ```

4. Add method to start location update
    ```
    @Override
    public void onConnected(@Nullable Bundle bundle) {
        Log.d(TAG, "Location service connected");
        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    ActivityCompat#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for ActivityCompat#requestPermissions for more details.
            return;
        }
        Location location = LocationServices.FusedLocationApi.getLastLocation(mGoogleApiClient);
        if (location != null) {
            handleNewLocation(location);
        }
        //start Location Update
        startLocationUpdate();
    }

    //method for start location update
    private void startLocationUpdate() {
        if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    ActivityCompat#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for ActivityCompat#requestPermissions for more details.
            return;
        }
        //start location update
        LocationServices.FusedLocationApi.requestLocationUpdates(mGoogleApiClient, mLocationRequest, this);
    }
    ```

5. Start and Stop Location Update
    Consider whether you want to stop the location updates when the activity is no longer in focus, such as when the user switches to another app or to a different activity in the same app. This can be handy to reduce power consumption, provided the app doesn't need to collect information even when it's running in the background.
    ```
    @Override
    protected void onResume() {
        super.onResume();
        mGoogleApiClient.connect();
        if(mGoogleApiClient.isConnected()){
            //start location update
            startLocationUpdate();
        }
    }

    @Override
    protected void onPause() {
        super.onPause();
        if(mGoogleApiClient.isConnected()){
            mGoogleApiClient.disconnect();
        }
        //stop location update
        try{
            LocationServices.FusedLocationApi.removeLocationUpdates(mGoogleApiClient,this);
        }catch (Exception e){
            Log.e(TAG,e.toString());
        }
    }
    ```