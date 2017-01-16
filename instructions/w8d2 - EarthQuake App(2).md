![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 8 - Day 2 (Earthquake App part 2)

### <a name="lab11"></a>1. Add EarthQuake Intent to show webpage about eartquake

Modify the app so that clicking on a list item goes to detailed webpage about that eartquake

1. Store url as String in EartQuake class

    ```
    //url website
    private String mUrl;

    //contuctor
    public EarthQuake(double magnitude, String location, long TImeMillisecond, String url) {
        mMagnitude = magnitude;
        mLocation = location;
        mTImeMillisecond = TImeMillisecond;
        mUrl = url;
    }

    //getter url
    public String getUrl() {
        return mUrl;
    }

    //setter url
    public void setUrl(String url) {
        mUrl = url;
    }

    ```

2. In QueryUtils class, extract url as String when parsing the JSON response
    ```
    String url = properties.getString("url");

    //get data url
    earthQuakes.add(new EarthQuake(mag,place,time,url));
    ```

3.  Add intent to open url website to MainActivity. When user click on item of Eartquake list, app will open a website of eartquake detail

    ```
    //set on item click listener
    earthquakeListView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
        @Override
        public void onItemClick(AdapterView<?> adapterView, View view, int position, long l) {
            //mendapatkan data eratquke
            EarthQuake currentEarthQuake = adapter.getItem(position);

            //convert String url menjadi URI object
            Uri earthQuakeUri = Uri.parse(currentEarthQuake.getUrl());

            //create new intent untuk membuka halaman website dari eartquake
            Intent websiteIntent = new Intent(Intent.ACTION_VIEW,earthQuakeUri);

            //mengirim intent untuk membuka website
            startActivity(websiteIntent);
        }
    });
    ```

---

### <a name="lab12"></a>2. Prepare App to get data from USGS website

1. Add permission to allow app acces internet in manifest file. 
    ```
    <uses-permission android:name="android.permission.INTERNET"/>
    ```

2. Add method to create URL object from string in QueryUtils.java 
   ```
   //create url object from string
    public static URL createURL(String stringUrl){
        URL url = null;
        try {
            url = new URL(stringUrl);
        } catch (MalformedURLException e) {
            Log.e(TAG,e.toString());
        }
        return url;
    }
   ```

3. Add method to get data from server USGS in QueryUtils.java
    ```
    //membuat http request dan me return data string dari USGS
    public static String makeHttoRequest(URL url){
        String jsonResponse = "";

        //jika url null, maka tidak akan diproses
        if(url == null){
            return jsonResponse;
        }

        HttpURLConnection urlConnection = null;
        InputStream inputStream = null;

        try {
            //open connection
            urlConnection = (HttpURLConnection) url.openConnection();
            //set maksimal waktu untuk mendapatkan data dari inter
            urlConnection.setReadTimeout(10000);
            urlConnection.setConnectTimeout(15000);
            urlConnection.setRequestMethod("GET");
            urlConnection.connect();

            //jika request suksess (response code 200)
            //maka baca inputr stream dan parsing response
            if (urlConnection.getResponseCode() == 200){
                inputStream = urlConnection.getInputStream();
                jsonResponse = readFromStream(inputStream);
            }else{
                Log.e(TAG,"Error response");
            }

        } catch (IOException e) {
            e.printStackTrace();
            Log.e(TAG,e.toString());
        }finally {
            if(urlConnection != null){
                urlConnection.disconnect();
            }
            if(inputStream != null){
                //clode input stream
                try {
                    inputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                    Log.e(TAG,e.toString());
                }
            }
        }

        return jsonResponse;

    }

    //konversi input stream ke string yang memuat data json dari server
    private static String readFromStream(InputStream inputStream) throws IOException {
        StringBuilder output = new StringBuilder();
        if(inputStream != null){
            InputStreamReader inputStreamReader = new InputStreamReader(inputStream, Charset.forName("UTF-8"));
            BufferedReader reader = new BufferedReader(inputStreamReader);
            String line = reader.readLine();
            while (line != null){
                output.append(line);
                line = reader.readLine();
            }
        }
        return output.toString();
    }
    ```

---

### <a name="lab13"></a>3.Get data from USGS website use Asyntask 

1. Create variable to store url to get data from usgs in MainActivity 
    ```
    private static final String USGS_REQUEST_URL = "http://earthquake.usgs.gov/fdsnws/event/1/query?format=geojson&orderby=time&minmag=4&limit=30";
    ```

2. Change variable earthquakeListView and adapter to global variable

3.  Add EmptyArray List in MainActivity and set to Adapter
    ```
    public class MainActivity extends AppCompatActivity {

    ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);


        //membuat data dummy dari eartquake
        ArrayList<EarthQuake> earthQuakes = new ArrayList<EarthQuake>();

        //listview
        earthquakeListView = (ListView) findViewById(R.id.list);

        //membuat adapter
        adapter = new EarthQuakeAdapter(this,earthQuakes);

        //set Adapter
        earthquakeListView.setAdapter(adapter);
    
        ...

    ```


3. Change extractEarthquakes() method that accepts a single parameter and parsing JSON dari parameter
    ```
    
    ...
    //metthod untuk mengkonvert data json dari USGS menjadi list
    public static ArrayList<EarthQuake> extractEarthquakes(String responseJson){

        //membuat arraylist kosong
        ArrayList<EarthQuake> earthQuakes = new ArrayList<>();

        //parsing json
        try {
            //membuat json object
            JSONObject root = new JSONObject(responseJson);
            JSONArray features = root.getJSONArray("features");
    ...

    ```

5. Create inner class in the main activity which is a sub class of asyntask to download data from USGS server
    ```
    private class DownloadTaskEartQuake extends AsyncTask<Void,Void,ArrayList<EarthQuake>>{

        @Override
        protected ArrayList<EarthQuake> doInBackground(Void... voids) {

            //set up URL
            URL url = QueryUtils.createURL(USGS_REQUEST_URL);

            //membuat http request ke URL dan menerima response JSON
            String jsonResponse = null;

            jsonResponse = QueryUtils.makeHttoRequest(url);

            // convert respone dalam bentuk jason menjadi array list
            ArrayList<EarthQuake> earthQuakes = QueryUtils.extraEartquake(jsonResponse);

            //return list
            return earthQuakes;

        }

        @Override
        protected void onPostExecute(ArrayList<EarthQuake> earthQuakes) {
            super.onPostExecute(earthQuakes);
            //add eartquake ke list view
            for(int i = 0;i < earthQuakes.size(); i++){
                adapter.add(earthQuakes.get(i));
            }
        }
    }
    ```

6. Execute asyntask in OnCreate Method 
    ```
    new DownloadTaskEartQuake().execute();
    ```










