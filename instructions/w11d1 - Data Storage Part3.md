![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 11 - Day 1 (Pets App Part #2)

### <a name="lab11"></a>1. Add Content Provider To Pets App

1. Create PetProvider class which extends Content Provider
    ```
    public class PetProvider extends ContentProvider {

    
        @Override
        public boolean onCreate() {
            return false;
        }

        @Nullable
        @Override
        public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {
            return null;
        }

        @Nullable
        @Override
        public String getType(Uri uri) {
            return null;
        }

        @Nullable
        @Override
        public Uri insert(Uri uri, ContentValues values) {
            return null;
        }

        @Override
        public int delete(Uri uri, String selection, String[] selectionArgs) {
            return 0;
        }

        @Override
        public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {
            return 0;
        }
    }
    ```

2. Declare PetProvider as an application component of the Pets app in AndroidManifest.xml inside <application></application>
    ```
    <provider
            android:authorities="com.example.android.pets"
            android:name=".PetProvider"
            android:exported="false"/>
    ```

3. Add Global Variable PetDbHelper inside PetProvider 
    ```
    /** Database helper object */
    private PetDbHelper mDbHelper;
    ```

4. Inisiate PetDbHelper variable inside OnCreate() method in PetProvider
    ```
    @Override
    public boolean onCreate() {
        mDbHelper = new PetDbHelper(getContext());
        return true;
    }
    ```

5. Add String Item to string.xml
    ```
    <!-- Toast message in editor when new pet has been successfully inserted [CHAR LIMIT=NONE] -->
    <string name="editor_insert_pet_successful">Pet saved</string>

    <!-- Toast message in editor when new pet has failed to be inserted [CHAR LIMIT=NONE] -->
    <string name="editor_insert_pet_failed">Error with saving pet</string>
    ```

---

### <a name="lab11"></a>2. Add Content URI To Pets App

1. Add Constant Variable inside PetContract class 
    ```
    /**
     * The "Content authority" is a name for the entire content provider, similar to the
     * relationship between a domain name and its website.  A convenient string to use for the
     * content authority is the package name for the app, which is guaranteed to be unique on the
     * device.
     */
    public static final String CONTENT_AUTHORITY = "com.example.android.pets";

    /**
     * Use CONTENT_AUTHORITY to create the base of all URI's which apps will use to contact
     * the content provider.
     */
    public static final Uri BASE_CONTENT_URI = Uri.parse("content://" + CONTENT_AUTHORITY);

    /**
     * Possible path (appended to base content URI for possible URI's)
     * For instance, content://com.example.android.pets/pets/ is a valid path for
     * looking at pet data. content://com.example.android.pets/staff/ will fail,
     * as the ContentProvider hasn't been given any information on what to do with "staff".
     */
    public static final String PATH_PETS = "pets";
    ```

2.  Inside PetEntry classes in the contract, create a full URI for the class as a constant called CONTENT_URI 

    ```
    public static final class PetEntry implements BaseColumns {

        /** The content URI to access the pet data in the provider */
        public static final Uri CONTENT_URI = Uri.withAppendedPath(BASE_CONTENT_URI, PATH_PETS);

        ...
    ```

3. Add URIMatcher To ContentProvider
    ```
    public class PetProvider extends ContentProvider { 
        /** URI matcher code for the content URI for the pets table */
        private static final int PETS = 100;

        /** URI matcher code for the content URI for a single pet in the pets table */
        private static final int PET_ID = 101;

        /**
        * UriMatcher object to match a content URI to a corresponding code.
        * The input passed into the constructor represents the code to return for the root URI.
        * It's common to use NO_MATCH as the input for this case.
        */
        private static final UriMatcher sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);

        // Static initializer. This is run the first time anything is called from this class.
        static {
            // The calls to addURI() go here, for all of the content URI patterns that the provider
            // should recognize. All paths added to the UriMatcher have a corresponding code to return
            // when a match is found.

            // The content URI of the form "content://com.example.android.pets/pets" will map to the
            // integer code {@link #PETS}. This URI is used to provide access to MULTIPLE rows
            // of the pets table.
            sUriMatcher.addURI(PetContract.CONTENT_AUTHORITY, PetContract.PATH_PETS, PETS);

            // The content URI of the form "content://com.example.android.pets/pets/#" will map to the
            // integer code {@link #PET_ID}. This URI is used to provide access to ONE single row
            // of the pets table.
            //
            // In this case, the "#" wildcard is used where "#" can be substituted for an integer.
            // For example, "content://com.example.android.pets/pets/3" matches, but
            // "content://com.example.android.pets/pets" (without a number at the end) doesn't match.
            sUriMatcher.addURI(PetContract.CONTENT_AUTHORITY, PetContract.PATH_PETS + "/#", PET_ID);
        }
    ...
    ```

---

### <a name="lab11"></a>3. Implements Content Provider Query() Method

1. Replace query() method in PetProvider class with following code : 
    ```
    @Override
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs,
                        String sortOrder) {
        // Get readable database
        SQLiteDatabase database = mDbHelper.getReadableDatabase();

        // This cursor will hold the result of the query
        Cursor cursor;

        // Figure out if the URI matcher can match the URI to a specific code
        int match = sUriMatcher.match(uri);
        switch (match) {
            case PETS:
                // For the PETS code, query the pets table directly with the given
                // projection, selection, selection arguments, and sort order. The cursor
                // could contain multiple rows of the pets table.
                cursor = database.query(PetEntry.TABLE_NAME, projection, selection, selectionArgs,
                        null, null, sortOrder);
                break;
            case PET_ID:
                // For the PET_ID code, extract out the ID from the URI.
                // For an example URI such as "content://com.example.android.pets/pets/3",
                // the selection will be "_id=?" and the selection argument will be a
                // String array containing the actual ID of 3 in this case.
                //
                // For every "?" in the selection, we need to have an element in the selection
                // arguments that will fill in the "?". Since we have 1 question mark in the
                // selection, we have 1 String in the selection arguments' String array.
                selection = PetEntry._ID + "=?";
                selectionArgs = new String[] { String.valueOf(ContentUris.parseId(uri)) };

                // This will perform a query on the pets table where the _id equals 3 to return a
                // Cursor containing that row of the table.
                cursor = database.query(PetEntry.TABLE_NAME, projection, selection, selectionArgs,
                        null, null, sortOrder);
                break;
            default:
                throw new IllegalArgumentException("Cannot query unknown URI " + uri);
        }
        return cursor;
    }
    ```

2. Remove code that directly interats with PetDbHelper and SQLiteDatabase in CatalogActivity displayDatabaseInfo() method 

3. Instead, call the ContentResolver query() method (which in turn, will call the PetProvider query() method) and receive a Cursor result. Use same projection as before . 
    ```
    public class CatalogActivity extends AppCompatActivity {

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_catalog);

            // Setup FAB to open EditorActivity
            FloatingActionButton fab = (FloatingActionButton) findViewById(R.id.fab);
            fab.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    Intent intent = new Intent(CatalogActivity.this, EditorActivity.class);
                    startActivity(intent);
                }
            });
        }

        @Override
        protected void onStart() {
            super.onStart();
            displayDatabaseInfo();
        }

        /**
        * Temporary helper method to display information in the onscreen TextView about the state of
        * the pets database.
        */
        private void displayDatabaseInfo() {
            // Define a projection that specifies which columns from the database
            // you will actually use after this query.
            String[] projection = {
                    PetEntry._ID,
                    PetEntry.COLUMN_PET_NAME,
                    PetEntry.COLUMN_PET_BREED,
                    PetEntry.COLUMN_PET_GENDER,
                    PetEntry.COLUMN_PET_WEIGHT };

            // Perform a query on the provider using the ContentResolver.
            // Use the {@link PetEntry#CONTENT_URI} to access the pet data.
            Cursor cursor = getContentResolver().query(
                    PetEntry.CONTENT_URI,   // The content URI of the words table
                    projection,             // The columns to return for each row
                    null,                   // Selection criteria
                    null,                   // Selection criteria
                    null);
        ...
    ```

---

### <a name="lab11"></a>4. Implements Content Provider Insert() Method

1. Replace insert() method in PetProvider class with following code : 
    ```
    @Override
    public Uri insert(Uri uri, ContentValues contentValues) {
        final int match = sUriMatcher.match(uri);
        switch (match) {
            case PETS:
                return insertPet(uri, contentValues);
            default:
                throw new IllegalArgumentException("Insertion is not supported for " + uri);
        }
    }

    /**
    * Insert a pet into the database with the given content values. Return the new content URI
    * for that specific row in the database.
    */
    private Uri insertPet(Uri uri, ContentValues values) {
        
        // TODO: Insert a new pet into the pets database table with the given ContentValues

        // Once we know the ID of the new row in the table,
        // return the new URI with the ID appended to the end of it
        return ContentUris.withAppendedId(uri, id);
    }
    ```

2.  Add helper method that check if gender is valid in PetEntry class
    ```
    /**
    * Returns whether or not the given gender is {@link #GENDER_UNKNOWN}, {@link #GENDER_MALE},
    * or {@link #GENDER_FEMALE}.
    */
    public static boolean isValidGender(int gender) {
        if (gender == GENDER_UNKNOWN || gender == GENDER_MALE || gender == GENDER_FEMALE) {
            return true;
        }
        return false;
    }
    ```

3.  Add statement for insert data inside insertPet() method 
    ```
    private Uri insertPet(Uri uri, ContentValues values) {
        // Check that the name is not null
        String name = values.getAsString(PetEntry.COLUMN_PET_NAME);
        if (name == null) {
            throw new IllegalArgumentException("Pet requires a name");
        }

        // Check that the gender is valid
        Integer gender = values.getAsInteger(PetEntry.COLUMN_PET_GENDER);
        if (gender == null || !PetEntry.isValidGender(gender)) {
            throw new IllegalArgumentException("Pet requires valid gender");
        }

        // If the weight is provided, check that it's greater than or equal to 0 kg
        Integer weight = values.getAsInteger(PetEntry.COLUMN_PET_WEIGHT);
        if (weight != null && weight < 0) {
            throw new IllegalArgumentException("Pet requires valid weight");
        }

        // No need to check the breed, any value is valid (including null).

        // Get writeable database
        SQLiteDatabase database = mDbHelper.getWritableDatabase();

        // Insert the new pet with the given values
        long id = database.insert(PetEntry.TABLE_NAME, null, values);
        // If the ID is -1, then the insertion failed. Log an error and return null.
        if (id == -1) {
            Log.e(LOG_TAG, "Failed to insert row for " + uri);
            return null;
        }

        // Return the new URI with the ID (of the newly inserted row) appended at the end
        return ContentUris.withAppendedId(uri, id);
    }
    ```

4.  Remove code in CatalogActivity and Editor Activity that queries database directly (insert data).  

5.  Instead, call the ContentResolver insert() method (which in turn, will call the PetProvider insert() method) and receive a URI result.
    
    In CatalogActivity.class
    ```
    private void insertPet() {
        // Create a ContentValues object where column names are the keys,
        // and Toto's pet attributes are the values.
        ContentValues values = new ContentValues();
        values.put(PetEntry.COLUMN_PET_NAME, "Toto");
        values.put(PetEntry.COLUMN_PET_BREED, "Terrier");
        values.put(PetEntry.COLUMN_PET_GENDER, PetEntry.GENDER_MALE);
        values.put(PetEntry.COLUMN_PET_WEIGHT, 7);

        // Insert a new row for Toto into the provider using the ContentResolver.
        // Use the {@link PetEntry#CONTENT_URI} to indicate that we want to insert
        // into the pets database table.
        // Receive the new content URI that will allow us to access Toto's data in the future.
        Uri newUri = getContentResolver().insert(PetEntry.CONTENT_URI, values);
    }
    ```

    In EditorActivity.class
    ```
    /**
     * Get user input from editor and save new pet into database.
     */
    private void insertPet() {
        // Read from input fields
        // Use trim to eliminate leading or trailing white space
        String nameString = mNameEditText.getText().toString().trim();
        String breedString = mBreedEditText.getText().toString().trim();
        String weightString = mWeightEditText.getText().toString().trim();
        int weight = Integer.parseInt(weightString);

        // Create a ContentValues object where column names are the keys,
        // and pet attributes from the editor are the values.
        ContentValues values = new ContentValues();
        values.put(PetEntry.COLUMN_PET_NAME, nameString);
        values.put(PetEntry.COLUMN_PET_BREED, breedString);
        values.put(PetEntry.COLUMN_PET_GENDER, mGender);
        values.put(PetEntry.COLUMN_PET_WEIGHT, weight);

        // Insert a new pet into the provider, returning the content URI for the new pet.
        Uri newUri = getContentResolver().insert(PetEntry.CONTENT_URI, values);

        // Show a toast message depending on whether or not the insertion was successful
        if (newUri == null) {
            // If the new content URI is null, then there was an error with insertion.
            Toast.makeText(this, getString(R.string.editor_insert_pet_failed),
                    Toast.LENGTH_SHORT).show();
        } else {
            // Otherwise, the insertion was successful and we can display a toast.
            Toast.makeText(this, getString(R.string.editor_insert_pet_successful),
                    Toast.LENGTH_SHORT).show();
        }
    }
    ```

---

### <a name="lab15"></a>5. Implements Content Provider Update() Method

1. Replace Update() method in PetProvider and add updatePet() helper method 
   ```
   @Override
    public int update(Uri uri, ContentValues contentValues, String selection,
                    String[] selectionArgs) {
        final int match = sUriMatcher.match(uri);
        switch (match) {
            case PETS:
                return updatePet(uri, contentValues, selection, selectionArgs);
            case PET_ID:
                // For the PET_ID code, extract out the ID from the URI,
                // so we know which row to update. Selection will be "_id=?" and selection
                // arguments will be a String array containing the actual ID.
                selection = PetEntry._ID + "=?";
                selectionArgs = new String[] { String.valueOf(ContentUris.parseId(uri)) };
                return updatePet(uri, contentValues, selection, selectionArgs);
            default:
                throw new IllegalArgumentException("Update is not supported for " + uri);
        }
    }

    /**
    * Update pets in the database with the given content values. Apply the changes to the rows
    * specified in the selection and selection arguments (which could be 0 or 1 or more pets).
    * Return the number of rows that were successfully updated.
    */
    private int updatePet(Uri uri, ContentValues values, String selection, String[] selectionArgs) {
    
        // TODO: Update the selected pets in the pets database table with the given ContentValues

        // TODO: Return the number of rows that were affected
        return 0;
    }
   ``` 

2. Add Statement for update data inside updatePet() method
    ```
    private int updatePet(Uri uri, ContentValues values, String selection, String[] selectionArgs) {
        // If the {@link PetEntry#COLUMN_PET_NAME} key is present,
        // check that the name value is not null.
        if (values.containsKey(PetEntry.COLUMN_PET_NAME)) {
            String name = values.getAsString(PetEntry.COLUMN_PET_NAME);
            if (name == null) {
                throw new IllegalArgumentException("Pet requires a name");
            }
        }

        // If the {@link PetEntry#COLUMN_PET_GENDER} key is present,
        // check that the gender value is valid.
        if (values.containsKey(PetEntry.COLUMN_PET_GENDER)) {
            Integer gender = values.getAsInteger(PetEntry.COLUMN_PET_GENDER);
            if (gender == null || !PetEntry.isValidGender(gender)) {
                throw new IllegalArgumentException("Pet requires valid gender");
            }
        }

        // If the {@link PetEntry#COLUMN_PET_WEIGHT} key is present,
        // check that the weight value is valid.
        if (values.containsKey(PetEntry.COLUMN_PET_WEIGHT)) {
            // Check that the weight is greater than or equal to 0 kg
            Integer weight = values.getAsInteger(PetEntry.COLUMN_PET_WEIGHT);
            if (weight != null && weight < 0) {
                throw new IllegalArgumentException("Pet requires valid weight");
            }
        }

        // No need to check the breed, any value is valid (including null).

        // If there are no values to update, then don't try to update the database
        if (values.size() == 0) {
            return 0;
        }

        // Otherwise, get writeable database to update the data
        SQLiteDatabase database = mDbHelper.getWritableDatabase();

        // Returns the number of database rows affected by the update statement
        return database.update(PetEntry.TABLE_NAME, values, selection, selectionArgs);
    }
    ```

### <a name="lab16"></a>6. Implements Content Provider Delete() Method

1. Add Statement for delete data inside delete() method
    ```
    @Override
    public int delete(Uri uri, String selection, String[] selectionArgs) {
        // Get writeable database
        SQLiteDatabase database = mDbHelper.getWritableDatabase();

        final int match = sUriMatcher.match(uri);
        switch (match) {
            case PETS:
                // Delete all rows that match the selection and selection args
                return database.delete(PetEntry.TABLE_NAME, selection, selectionArgs);
            case PET_ID:
                // Delete a single row given by the ID in the URI
                selection = PetEntry._ID + "=?";
                selectionArgs = new String[] { String.valueOf(ContentUris.parseId(uri)) };
                return database.delete(PetEntry.TABLE_NAME, selection, selectionArgs);
            default:
                throw new IllegalArgumentException("Deletion is not supported for " + uri);
        }
    }
    ```

### <a name="lab17"></a>7. Implements Content Provider getType() Method

1. Add Constant variable to PetEntry class
    ```
     /**
         * The MIME type of the {@link #CONTENT_URI} for a list of pets.
         */
        public static final String CONTENT_LIST_TYPE =
                ContentResolver.CURSOR_DIR_BASE_TYPE + "/" + CONTENT_AUTHORITY + "/" + PATH_PETS;

        /**
         * The MIME type of the {@link #CONTENT_URI} for a single pet.
         */
        public static final String CONTENT_ITEM_TYPE =
                ContentResolver.CURSOR_ITEM_BASE_TYPE + "/" + CONTENT_AUTHORITY + "/" + PATH_PETS;
    ```

2. Replace getType() method with following code : 
    ```
    @Override
    public String getType(Uri uri) {
        final int match = sUriMatcher.match(uri);
        switch (match) {
            case PETS:
                return PetEntry.CONTENT_LIST_TYPE;
            case PET_ID:
                return PetEntry.CONTENT_ITEM_TYPE;
            default:
                throw new IllegalStateException("Unknown URI " + uri + " with match " + match);
        }
    }
    ```





