![Makers Institute](https://makersinstitute.id/img/logo-makersinstitute.png)

# Hands on Lab Week 11 - Day 2 (Pets App Part #3)

### 1. Create PetCursorAdapter to display list of pets in ListView

1. Change `TextView` layout inside the `CatalogActivity` into a `ListView`

	```xml
    <ListView
        android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
    ```
2. Create item view for `ListView`

	```xml
    <?xml version="1.0" encoding="utf-8"?>
    <!-- Copyright (C) 2016 The Android Open Source Project
         Licensed under the Apache License, Version 2.0 (the "License");
         you may not use this file except in compliance with the License.
         You may obtain a copy of the License at
              http://www.apache.org/licenses/LICENSE-2.0
         Unless required by applicable law or agreed to in writing, software
         distributed under the License is distributed on an "AS IS" BASIS,
         WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
         See the License for the specific language governing permissions and
         limitations under the License.
    -->
    <!-- Layout for a single list item in the list of pets -->
    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="@dimen/activity_margin">

        <TextView
            android:id="@+id/name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:fontFamily="sans-serif-medium"
            android:textAppearance="?android:textAppearanceMedium"
            android:textColor="#2B3D4D"  />

        <TextView
            android:id="@+id/summary"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:fontFamily="sans-serif"
            android:textAppearance="?android:textAppearanceSmall"
            android:textColor="#AEB6BD"  />
    </LinearLayout>
    ```

3. Create a `CursorAdapter` class for `ListView`
	
    ```java
    public class PetCursorAdapter extends CursorAdapter {

      public PetCursorAdapter(Context context, Cursor c) {
          super(context, c, 0 /* flags */);
      }

      @Override
      public View newView(Context context, Cursor cursor, ViewGroup parent) {
          // Inflate a list item view using the layout specified in list_item.xml
          return LayoutInflater.from(context).inflate(R.layout.list_item, parent, false);
      }

      @Override
      public void bindView(View view, Context context, Cursor cursor) {
          // Find individual views that we want to modify in the list item layout
          TextView nameTextView = (TextView) view.findViewById(R.id.name);
          TextView summaryTextView = (TextView) view.findViewById(R.id.summary);

          // Find the columns of pet attributes that we're interested in
          int nameColumnIndex = cursor.getColumnIndex(PetEntry.COLUMN_PET_NAME);
          int breedColumnIndex = cursor.getColumnIndex(PetEntry.COLUMN_PET_BREED);

          // Read the pet attributes from the Cursor for the current pet
          String petName = cursor.getString(nameColumnIndex);
          String petBreed = cursor.getString(breedColumnIndex);

          // Update the TextViews with the attributes for the current pet
          nameTextView.setText(petName);
          summaryTextView.setText(petBreed);
      }
	}
    ```
    
4. Delete the `TextView` and the `try/finally` block inside the `displayDatabaseInfo()` and change it into a ListView, Adapter, and assing the Adapter into ListView

	```java
    // Find the ListView which will be populated with the pet data
    ListView petListView = (ListView) findViewById(R.id.list);

    // Setup an Adapter to create a list item for each row of pet data in the Cursor.
    PetCursorAdapter adapter = new PetCursorAdapter(this, cursor);

    // Attach the adapter to the ListView.
    petListView.setAdapter(adapter);
    ```

### 2. Add empty view to the ListView if there is no data

1. Add another layout below the `<ListView/>`

	```xml
    <!-- Empty view for the list -->
    <RelativeLayout
        android:id="@+id/empty_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true">

        <ImageView
            android:id="@+id/empty_shelter_image"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerHorizontal="true"
            android:src="@drawable/ic_empty_shelter"/>

        <TextView
            android:id="@+id/empty_title_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_below="@+id/empty_shelter_image"
            android:layout_centerHorizontal="true"
            android:fontFamily="sans-serif-medium"
            android:paddingTop="16dp"
            android:text="@string/empty_view_title_text"
            android:textAppearance="?android:textAppearanceMedium"/>

        <TextView
            android:id="@+id/empty_subtitle_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_below="@+id/empty_title_text"
            android:layout_centerHorizontal="true"
            android:fontFamily="sans-serif"
            android:paddingTop="8dp"
            android:text="@string/empty_view_subtitle_text"
            android:textAppearance="?android:textAppearanceSmall"
            android:textColor="#A2AAB0"/>
    </RelativeLayout>
    ```

2. Assign the empty view into the code

	```java
    // Find the ListView which will be populated with the pet data
    ListView petListView = (ListView) findViewById(R.id.list);

    // Find and set empty view on the ListView, so that it only shows when the list has 0 items.
    View emptyView = findViewById(R.id.empty_view);
    petListView.setEmptyView(emptyView);
    ```

### 3. Switch to CursorLoader

1. Add an `implements` for `CatalogActivity` because we are going to use `Loader`

	```java
    public class CatalogActivity extends AppCompatActivity implements
        LoaderManager.LoaderCallbacks<Cursor> {}
    ```

2. Declare ID for `Loader` and the adapter

	```java
    /** Identifier for the pet data loader */
    private static final int PET_LOADER = 0;

    /** Adapter for the ListView */
    PetCursorAdapter mCursorAdapter;
    ```

3. Remove anything inside `onStart` since we are going to use Loader
4. Then initiate the `Loader` at `onCreate`

	```java
    // There is no pet data yet (until the loader finishes) so pass in null for the Cursor.
    mCursorAdapter = new PetCursorAdapter(this, null);
    petListView.setAdapter(mCursorAdapter);

    // Kick off the loader
    getLoaderManager().initLoader(PET_LOADER, null, this);
    ```

5. Next, we override the `onCreateLoader` method

	```java
    @Override
    public Loader<Cursor> onCreateLoader(int i, Bundle bundle) {
        // Define a projection that specifies the columns from the table we care about.
        String[] projection = {
                PetEntry._ID,
                PetEntry.COLUMN_PET_NAME,
                PetEntry.COLUMN_PET_BREED };

        // This loader will execute the ContentProvider's query method on a background thread
        return new CursorLoader(this,   // Parent activity context
                PetEntry.CONTENT_URI,   // Provider content URI to query
                projection,             // Columns to include in the resulting Cursor
                null,                   // No selection clause
                null,                   // No selection arguments
                null);                  // Default sort order
    }
    ```

6. Finally, add `onLoadFinish` and `onLoaderReset`

	```java
    @Override
    public void onLoadFinished(Loader<Cursor> loader, Cursor data) {
        // Update {@link PetCursorAdapter} with this new cursor containing updated pet data
        mCursorAdapter.swapCursor(data);
    }

    @Override
    public void onLoaderReset(Loader<Cursor> loader) {
        // Callback called when the data needs to be deleted
        mCursorAdapter.swapCursor(null);
    }
    ```

### 4. Modify ContentProvider so Loader refreshes data automatically

1. On `PetProvider` class, add following code inside `query` method before the return value

	```java
    // Set notification URI on the Cursor,
    // so we know what content URI the Cursor was created for.
    // If the data at this URI changes, then we know we need to update the Cursor.
    cursor.setNotificationUri(getContext().getContentResolver(), uri);
    ```

2. Also, add following code inside `insertPet` method, to let the provider's listener knows there is a change

	```java
    // Notify all listeners that the data has changed for the pet content URI
    getContext().getContentResolver().notifyChange(uri, null);
    ```

3. Then, change the return value of `updatePet` method

	```java
	// Perform the update on the database and get the number of rows affected
    int rowsUpdated = database.update(PetEntry.TABLE_NAME, values, selection, selectionArgs);

    // If 1 or more rows were updated, then notify all listeners that the data at the
    // given URI has changed
    if (rowsUpdated != 0) 
        getContext().getContentResolver().notifyChange(uri, null);
    }

    // Return the number of rows updated
    return rowsUpdated;
    ```

4. Now, inside `delete` method, add following code below getting readable database

	```java
    // Track the number of rows that were deleted
    int rowsDeleted;
    ```

5. Next, change `return` value on **both** cases with this,

	```java
    rowsDeleted = database.delete(PetEntry.TABLE_NAME, selection, selectionArgs);
    break;
    ```

6. Finally, after the `switch/case` statement, notify listener that data has been changed

	```java
    // If 1 or more rows were deleted, then notify all listeners that the data at the
    // given URI has changed
    if (rowsDeleted != 0) {
        getContext().getContentResolver().notifyChange(uri, null);
    }

    // Return the number of rows deleted
    return rowsDeleted;
    ```

### 5. Clicking on list item opens EditorActivity

1. Open `AndroidManifest.xml` then **delete** a line on `.EditorActivity` because we are going to something with the label later

	```xml
    android:label="@string/editor_activity_title_new_pet"
    ```

2. Now, add `OnItemClickListener` to the `ListView`'s item below setting the adapter, `petListView.setAdapter(mCursorAdapter);` on the `onCreate` method, so we are able to edit the pet's data

	```java
    // Setup the item click listener
    petListView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
        @Override
        public void onItemClick(AdapterView<?> adapterView, View view, int position, long id) {
            // Create new intent to go to {@link EditorActivity}
            Intent intent = new Intent(CatalogActivity.this, EditorActivity.class);

            // Form the content URI that represents the specific pet that was clicked on,
            // by appending the "id" (passed as input to this method) onto the
            // {@link PetEntry#CONTENT_URI}.
            // For example, the URI would be "content://com.example.android.pets/pets/2"
            // if the pet with ID 2 was clicked on.
            Uri currentPetUri = ContentUris.withAppendedId(PetEntry.CONTENT_URI, id);

            // Set the URI on the data field of the intent
            intent.setData(currentPetUri);

            // Launch the {@link EditorActivity} to display the data for the current pet.
            startActivity(intent);
        }
    });
    ```

3. Inside the `EditorActivity` class, read the intent, wheter it is a adding a new pet, or editing an existing pet, then set the activity's title. Add following code inside `onCreate` below the `setContentView()`

	```java
    // Examine the intent that was used to launch this activity,
    // in order to figure out if we're creating a new pet or editing an existing one.
    Intent intent = getIntent();
    Uri currentPetUri = intent.getData();

    // If the intent DOES NOT contain a pet content URI, then we know that we are
    // creating a new pet.
    if (currentPetUri == null) {
        // This is a new pet, so change the app bar to say "Add a Pet"
        setTitle(getString(R.string.editor_activity_title_new_pet));
    } else {
        // Otherwise this is an existing pet, so change app bar to say "Edit Pet"
        setTitle(getString(R.string.editor_activity_title_edit_pet));
    }
    ```

4. Don't forget to add needed string into `strings.xml`

### 6. Load existing pet data from database

1. First, we implements to the activity, and add loader identifier and a new variable for URI

	```java
    public class EditorActivity extends AppCompatActivity implements
        LoaderManager.LoaderCallbacks<Cursor> {

        /** Identifier for the pet data loader */
        private static final int EXISTING_PET_LOADER = 0;

        /** Content URI for the existing pet (null if it's a new pet) */
        private Uri mCurrentPetUri;
    ```

2. Second, change the Uri declaration `Uri currentPetUri = intent.getData();`, with checking the `mContentPetUri;` from intent.

	```java
    mCurrentPetUri = intent.getData();
    ```

3. Instead of checking `(currentPetUri == null)`, change it into this, to check if the intent contains data or not.

	```java
    (mCurrentPetUri == null)
    ```

4. If the data is not null, so it goes to `else` statement, we initialize the loader

	```java
    // Initialize a loader to read the pet data from the database
    // and display the current values in the editor
    getLoaderManager().initLoader(EXISTING_PET_LOADER, null, this);
    ```

5. As we have done before, we override the `onCreateLoader` to load data from `CursorLoader`

	```java
    @Override
    public Loader<Cursor> onCreateLoader(int i, Bundle bundle) {
        // Since the editor shows all pet attributes, define a projection that contains
        // all columns from the pet table
        String[] projection = {
                PetEntry._ID,
                PetEntry.COLUMN_PET_NAME,
                PetEntry.COLUMN_PET_BREED,
                PetEntry.COLUMN_PET_GENDER,
                PetEntry.COLUMN_PET_WEIGHT };

        // This loader will execute the ContentProvider's query method on a background thread
        return new CursorLoader(this,   // Parent activity context
                mCurrentPetUri,         // Query the content URI for the current pet
                projection,             // Columns to include in the resulting Cursor
                null,                   // No selection clause
                null,                   // No selection arguments
                null);                  // Default sort order
    }
    ```

6. Then, process the data on the `onLoadFinished`

	```java
    @Override
    public void onLoadFinished(Loader<Cursor> loader, Cursor cursor) {
        // Bail early if the cursor is null or there is less than 1 row in the cursor
        if (cursor == null || cursor.getCount() < 1) {
            return;
        }

        // Proceed with moving to the first row of the cursor and reading data from it
        // (This should be the only row in the cursor)
        if (cursor.moveToFirst()) {
            // Find the columns of pet attributes that we're interested in
            int nameColumnIndex = cursor.getColumnIndex(PetEntry.COLUMN_PET_NAME);
            int breedColumnIndex = cursor.getColumnIndex(PetEntry.COLUMN_PET_BREED);
            int genderColumnIndex = cursor.getColumnIndex(PetEntry.COLUMN_PET_GENDER);
            int weightColumnIndex = cursor.getColumnIndex(PetEntry.COLUMN_PET_WEIGHT);

            // Extract out the value from the Cursor for the given column index
            String name = cursor.getString(nameColumnIndex);
            String breed = cursor.getString(breedColumnIndex);
            int gender = cursor.getInt(genderColumnIndex);
            int weight = cursor.getInt(weightColumnIndex);

            // Update the views on the screen with the values from the database
            mNameEditText.setText(name);
            mBreedEditText.setText(breed);
            mWeightEditText.setText(Integer.toString(weight));

            // Gender is a dropdown spinner, so map the constant value from the database
            // into one of the dropdown options (0 is Unknown, 1 is Male, 2 is Female).
            // Then call setSelection() so that option is displayed on screen as the current selection.
            switch (gender) {
                case PetEntry.GENDER_MALE:
                    mGenderSpinner.setSelection(1);
                    break;
                case PetEntry.GENDER_FEMALE:
                    mGenderSpinner.setSelection(2);
                    break;
                default:
                    mGenderSpinner.setSelection(0);
                    break;
            }
        }
    }
    ```

7. Finally override the `onLoaderReset`

	```java
    @Override
    public void onLoaderReset(Loader<Cursor> loader) {
        // If the loader is invalidated, clear out all the data from the input fields.
        mNameEditText.setText("");
        mBreedEditText.setText("");
        mWeightEditText.setText("");
        mGenderSpinner.setSelection(0); // Select "Unknown" gender
    }
    ```

### 7. Save changes to existing pet if it already exists

1. Since we are able to edit pet, inside the `EditorActivity` we change the `insertPet()` method into `savePet()` method

	```java
    private void savePet() {
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

        // Determine if this is a new or existing pet by checking if mCurrentPetUri is null or not
        if (mCurrentPetUri == null) {
            // This is a NEW pet, so insert a new pet into the provider,
            // returning the content URI for the new pet.
            Uri newUri = getContentResolver().insert(PetEntry.CONTENT_URI, values);

            // Show a toast message depending on whether or not the insertion was successful.
            if (newUri == null) {
                // If the new content URI is null, then there was an error with insertion.
                Toast.makeText(this, getString(R.string.editor_insert_pet_failed),
                        Toast.LENGTH_SHORT).show();
            } else {
                // Otherwise, the insertion was successful and we can display a toast.
                Toast.makeText(this, getString(R.string.editor_insert_pet_successful),
                        Toast.LENGTH_SHORT).show();
            }
        } else {
            // Otherwise this is an EXISTING pet, so update the pet with content URI: mCurrentPetUri
            // and pass in the new ContentValues. Pass in null for the selection and selection args
            // because mCurrentPetUri will already identify the correct row in the database that
            // we want to modify.
            int rowsAffected = getContentResolver().update(mCurrentPetUri, values, null, null);

            // Show a toast message depending on whether or not the update was successful.
            if (rowsAffected == 0) {
                // If no rows were affected, then there was an error with the update.
                Toast.makeText(this, getString(R.string.editor_update_pet_failed),
                        Toast.LENGTH_SHORT).show();
            } else {
                // Otherwise, the update was successful and we can display a toast.
                Toast.makeText(this, getString(R.string.editor_update_pet_successful),
                        Toast.LENGTH_SHORT).show();
            }
        }
    }
    ```

2. Then, change the code which call the `insertPet()` into `savePet()` 
3. Don't forget to add needed string into `strings.xml`

### 8. Prevent crash with blank editor

1. Now our app are able to edit data, so in the `EditorActivity` we have two possibilities, if it was editing existing pet, the text editor wouldn't be empty. But, if it is a new pet, the text editor should be empty.
2. To prevent an error while saving the data on a new pet, we need to check if the text editor is not empty

	```java
    // Check if this is supposed to be a new pet
    // and check if all the fields in the editor are blank
    if (mCurrentPetUri == null &&
            TextUtils.isEmpty(nameString) && TextUtils.isEmpty(breedString) &&
            TextUtils.isEmpty(weightString) && mGender == PetEntry.GENDER_UNKNOWN) {
        // Since no fields were modified, we can return early without creating a new pet.
        // No need to create ContentValues and no need to do any ContentProvider operations.
        return;
    }
    ```

3. And if the user isn't provide a pet's weight, we should accomodate that possibilities by giving a default value, 0. First, remove the `int weight = Integer.parseInt(weightString);` line of code, then add this code before `values.put(PetEntry.COLUMN_PET_WEIGHT, weight);`

	```java
    // If the weight is not provided by the user, don't try to parse the string into an
    // integer value. Use 0 by default.
    int weight = 0;
    if (!TextUtils.isEmpty(weightString)) {
        weight = Integer.parseInt(weightString);
    }
    ```

### 9. Warn user about losing unsaved changes

1. While editing inside the text editor, we might accidentally press back button and the data might be loss. To prevent that, we add a confirmation dialog
2. First, declare a boolean flag to keeps track what is happening

	```java
	/** Boolean flag that keeps track of whether the pet has been edited (true) or not (false) */
    private boolean mPetHasChanged = false;
    ```

3. Then, we declare a `OnTouchListener` for when the user touch the text editor, and change the boolean flag

	```java
    /**
     * OnTouchListener that listens for any user touches on a View, implying that they are modifying
     * the view, and we change the mPetHasChanged boolean to true.
     */
    private View.OnTouchListener mTouchListener = new View.OnTouchListener() {
        @Override
        public boolean onTouch(View view, MotionEvent motionEvent) {
            mPetHasChanged = true;
            return false;
        }
    };
    ```

4. Now we assign that `OnTouchListener` into every `EditText`'s view

	```java
    // Setup OnTouchListeners on all the input fields, so we can determine if the user
    // has touched or modified them. This will let us know if there are unsaved changes
    // or not, if the user tries to leave the editor without saving.
    mNameEditText.setOnTouchListener(mTouchListener);
    mBreedEditText.setOnTouchListener(mTouchListener);
    mWeightEditText.setOnTouchListener(mTouchListener);
    mGenderSpinner.setOnTouchListener(mTouchListener);
    ```

5. Then, we create a method to pop an `AlertDialog` to conform user's action about the editing time

	```java
    /**
     * Show a dialog that warns the user there are unsaved changes that will be lost
     * if they continue leaving the editor.
     *
     * @param discardButtonClickListener is the click listener for what to do when
     *                                   the user confirms they want to discard their changes
     */
    private void showUnsavedChangesDialog(
            DialogInterface.OnClickListener discardButtonClickListener) {
        // Create an AlertDialog.Builder and set the message, and click listeners
        // for the postivie and negative buttons on the dialog.
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setMessage(R.string.unsaved_changes_dialog_msg);
        builder.setPositiveButton(R.string.discard, discardButtonClickListener);
        builder.setNegativeButton(R.string.keep_editing, new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
                // User clicked the "Keep editing" button, so dismiss the dialog
                // and continue editing the pet.
                if (dialog != null) {
                    dialog.dismiss();
                }
            }
        });

        // Create and show the AlertDialog
        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
    ```

6. After that, we will change the action when user press back button on the `Toolbar` so edit the `case android.id.home` into

	```java
    // If the pet hasn't changed, continue with navigating up to parent activity
    // which is the {@link CatalogActivity}.
    if (!mPetHasChanged) {
        NavUtils.navigateUpFromSameTask(EditorActivity.this);
        return true;
    }

    // Otherwise if there are unsaved changes, setup a dialog to warn the user.
    // Create a click listener to handle the user confirming that
    // changes should be discarded.
    DialogInterface.OnClickListener discardButtonClickListener =
        new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
            // User clicked "Discard" button, navigate to parent activity.
                NavUtils.navigateUpFromSameTask(EditorActivity.this);
            }
        };

    // Show a dialog that notifies the user they have unsaved changes
    showUnsavedChangesDialog(discardButtonClickListener);
    return true;
    ```

7. Then, if user press the phone's back button, we take care of that too by overriding the `onBackPressed()` method

	```java
    /**
     * This method is called when the back button is pressed.
     */
    @Override
    public void onBackPressed() {
        // If the pet hasn't changed, continue with handling back button press
        if (!mPetHasChanged) {
            super.onBackPressed();
            return;
        }

        // Otherwise if there are unsaved changes, setup a dialog to warn the user.
        // Create a click listener to handle the user confirming that changes should be discarded.
        DialogInterface.OnClickListener discardButtonClickListener =
                new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        // User clicked "Discard" button, close the current activity.
                        finish();
                    }
                };

        // Show dialog that there are unsaved changes
        showUnsavedChangesDialog(discardButtonClickListener);
    }
    ```
8. Add following line of code into `strings.xml`

	```xml
    <!-- Dialog message when user is leaving editor but hasn't saved changes [CHAR LIMIT=NONE] -->
    <string name="unsaved_changes_dialog_msg">Discard your changes and quit editing?</string>

    <!-- Dialog button text for the option to discard a user's changes [CHAR LIMIT=20] -->
    <string name="discard">Discard</string>

    <!-- Dialog button text for the option to keep editing the current pet [CHAR LIMIT=20] -->
    <string name="keep_editing">Keep Editing</string>

    ```

### 10. Hide delete menu option for new pets

1. Open `EditorActivity` class, we add `invalidateOptionsMenu();` if the `(mCurrentPetUri == null)`
2. Then override the `onPrepareOptionsMenu()` method

	```java
    /**
     * This method is called after invalidateOptionsMenu(), so that the
     * menu can be updated (some menu items can be hidden or made visible).
     */
    @Override
    public boolean onPrepareOptionsMenu(Menu menu) {
        super.onPrepareOptionsMenu(menu);
        // If this is a new pet, hide the "Delete" menu item.
        if (mCurrentPetUri == null) {
            MenuItem menuItem = menu.findItem(R.id.action_delete);
            menuItem.setVisible(false);
        }
        return true;
    }
    ```

### 11. Delete pet from editor menu item

1. Create a new method for deleting a pets from database inside `EditorActivity` class

	```java
    /**
     * Perform the deletion of the pet in the database.
     */
    private void deletePet() {
        // Only perform the delete if this is an existing pet.
        if (mCurrentPetUri != null) {
            // Call the ContentResolver to delete the pet at the given content URI.
            // Pass in null for the selection and selection args because the mCurrentPetUri
            // content URI already identifies the pet that we want.
            int rowsDeleted = getContentResolver().delete(mCurrentPetUri, null, null);

            // Show a toast message depending on whether or not the delete was successful.
            if (rowsDeleted == 0) {
                // If no rows were deleted, then there was an error with the delete.
                Toast.makeText(this, getString(R.string.editor_delete_pet_failed),
                        Toast.LENGTH_SHORT).show();
            } else {
                // Otherwise, the delete was successful and we can display a toast.
                Toast.makeText(this, getString(R.string.editor_delete_pet_successful),
                        Toast.LENGTH_SHORT).show();
            }
        }

        // Close the activity
        finish();
    }
    ```

2. Then, create a confirmation dialog when user wants to delete the pets, which will call the `deletePet()`

	```java
    /**
     * Prompt the user to confirm that they want to delete this pet.
     */
    private void showDeleteConfirmationDialog() {
        // Create an AlertDialog.Builder and set the message, and click listeners
        // for the postivie and negative buttons on the dialog.
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setMessage(R.string.delete_dialog_msg);
        builder.setPositiveButton(R.string.delete, new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
                // User clicked the "Delete" button, so delete the pet.
                deletePet();
            }
        });
        builder.setNegativeButton(R.string.cancel, new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
                // User clicked the "Cancel" button, so dismiss the dialog
                // and continue editing the pet.
                if (dialog != null) {
                    dialog.dismiss();
                }
            }
        });

        // Create and show the AlertDialog
        AlertDialog alertDialog = builder.create();
        alertDialog.show();
    }
    ```

3. Finally, add following code inside the delete `case`

	```java
    // Pop up confirmation dialog for deletion
    showDeleteConfirmationDialog();
    return true;
    ```
4. Add following string to `strings.xml`

	```xml
    <!-- Toast message in editor when current pet was successfully deleted [CHAR LIMIT=NONE] -->
    <string name="editor_delete_pet_successful">Pet deleted</string>

    <!-- Toast message in editor when current pet has failed to be deleted [CHAR LIMIT=NONE] -->
    <string name="editor_delete_pet_failed">Error with deleting pet</string>

    <!-- Dialog message to ask the user to confirm deleting the current pet [CHAR LIMIT=NONE] -->
    <string name="delete_dialog_msg">Delete this pet?</string>

    <!-- Dialog button text for the option to confirm deleting the current pet [CHAR LIMIT=20] -->
    <string name="delete">Delete</string>

    <!-- Dialog button text for the option to cancel deletion of the current pet [CHAR LIMIT=20] -->
    <string name="cancel">Cancel</string>
    ```

### 12. Delete all pets from catalog menu item

1. Create a new method inside `CatalogActivity` class for delete all pets

	```java
    /**
     * Helper method to delete all pets in the database.
     */
    private void deleteAllPets() {
        int rowsDeleted = getContentResolver().delete(PetEntry.CONTENT_URI, null, null);
        Log.v("CatalogActivity", rowsDeleted + " rows deleted from pet database");
    }
    ```

2. Then call that method from delete all `case` statement

	```java
    case R.id.action_delete_all_entries:
        deleteAllPets();
        return true;
    ```

### 13. Display "Unknown breed" for pets without breed

1. We handle if user doesn't write anything inside the breed's text editor by showing "Unknown breed"

	```java
    // If the pet breed is empty string or null, then use some default text
    // that says "Unknown breed", so the TextView isn't blank.
    if (TextUtils.isEmpty(petBreed)) {
        petBreed = context.getString(R.string.unknown_breed);
    }
    ```

2. Add the string into `strings.xml`

	```xml
    <!-- Label for the pet's breed if the breed is unknown [CHAR LIMIT=20] -->
    <string name="unknown_breed">Unknown breed</string>
    ```



# Congrats!