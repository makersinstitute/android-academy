![Makers Institute](../images/logo-makersinstitute.png)
-

# Week 8: Book Catalog App

## Project Overview
This project is a chance for you to combine and practice everything you learned so far. You will be making a book catalog app.

The goal is to design and create the structure of a Book Catalog app which would allow a user to get a list of published books on a given topic. You will be using the `google books api` in order to fetch results and display them to the user.

## Why this project?
In the most recent portion of the class, you learned about the web and about how to get data from an api. You also learned how to parse that data and display it to a user. This project gives you the ability to practice those skills, which will be key in developing any app which makes use of a backend server, real time data, or interactions over the internet.

## What will I learn?
This project is about combining various ideas and skills we've been practicing throughout the class. They include:
- Fetching data from an API
- Using an AsyncTask
- Parsing a JSON response
- Creating a list based on that data and displaying it to the user.

## Build Your Project

Here are some prefered order to build the app. If you are feeling stuck or confused, try to ask the class Instructor, class assistant, your friend, or our best friend, Google Search Engine.

1. Clone the Starter Code from [here](https://github.com/makersinstitute/BookCatalog-v1), choose `master` branch.
2. Let's take a look at the code, there are three `Java Class`, and two `XML Layout`
	- `MainActivity.class`
		- It's a place where everything happens. From accessing the layout, setting the `ListView` adapter, adding an `ClickListener`, getting data from API server (request data, read response data, and parse response data)
	- `Book.class`
		- Here lies the data model we want to have. For now, we only need to know the book's title, book's author(s), and book's description, so we set the constructor, setter, and getter.
	- `BookAdapter.class`
		- You already passed some project that have a `ListView` adapter. It's just the same. Try to understand line by line of the code.
	- `activity_main.xml`
		- We only have three components in the `MainActivity`, search bar, button for search, and a `ListView` for showing the result.
	- `book_item.xml`
		- This is the layout of the result's book inside `ListView`

3. Now, let's code!
	1. First, we declare every variable that needed. It's a global variable.
	2. Then, we declare the API endpoints. So this is the link for you to request data about book's catalog. `BOOK_REQUEST_URL`.
	3. Inside `onCreate` method
		1. We connect the view with the code using `findViewById`
		2. (Optional) we declare a `ProgressDialog`, so the screen will have a loading view when we request the data from API server.
		3.  Add `ClickListener` for search button. First, *show the loading view*, then call the request method. We use the `AsyncTask` here.
			```java
            BookAsyncTask task = new BookAsyncTask();
            task.execute();
            ```
     4. Next, we declared another class, as we called in previous code Snippet. Here is the code to request the data from API server.
     	`private class BookAsyncTask extends AsyncTask<URL, Void, ArrayList<Book>> {}`
     	1. First, we override the `doInBackground` method to fullfil what we need. 
     		```java
            @Override
        	protected ArrayList<Book> doInBackground(URL... urls) {}
            ```
           So we add another thing personalized, first we check the `EditText` if it's empty or not. If it is empty, we show the user that the `EditText` as the search keyword, can not be empty.
     		```java
            private String searchKeyword = keywordText.getText().toString();
            if(searchKeyword.length() == 0) {

                runOnUiThread(new Runnable() {
                    public void run() {
                        Toast.makeText(MainActivity.this, "Keywords can not be empty", Toast.LENGTH_SHORT).show();
                    }
                });

                return null;
            }
            ```
            
            Then, if its not empty, we will process the data text inside that. Since we are going to add this keyword into a URL, we will replace the space (" ") with a plus ("+"). (Take a look at [here](http://stackoverflow.com/questions/1634271/url-encoding-the-space-character-or-20))
            
            `searchKeyword = searchKeyword.replace(" ", "+");`
        
        	After replacing the space, we create a full-form of the URL, using string concatenation'
            
            `URL url = createUrl(BOOK_REQUEST_URL + searchKeyword); \\ method declared later`
            
            Then, we try to request data from the API server,
            
            ```java
            String jsonResponse = "";
            try {
                jsonResponse = makeHttpRequest(url); // the function will be declared later. `makeHttpRequest()` return String formated JSON.
            } catch (IOException e) {
                Log.e("MainActivity", "IOException", e); // if error happened, we log the error
            }
            ```
            
            Finally, we process the response to convert it into an `ArrayList<Book>` and return the converted data
            ```java
            ArrayList<Book> books = extractBookInfoFromJson(jsonResponse); // method declared later

            return books;
            ```
         2. After we have the return value from `doInBackground` which is a `ArrayList<Book>`, we process it inside the `onPostExecute`. We override it then update the `ListView` to show the response data. 
         	```java
            @Override
        	protected void onPostExecute(ArrayList<Book> bookList) {}
            ```
         	Before that, *dismiss the loading view*. Make sure to check the `bookList`, if its empty, then update `ListView` with empty data, if not, show the `bookList`
         	```java
            if (bookList == null) {
                bookAdapter = new BookAdapter(MainActivity.this, new ArrayList<Book>());
                listView.setAdapter(bookAdapter);

                return;
            }

            bookAdapter = new BookAdapter(MainActivity.this, bookList);
            listView.setAdapter(bookAdapter);
            ```
            
       5. Next, we declare the `private URL createUrl(String stringUrl) {}` method, for creating URL from String
       		```java
        	URL url = null;
        	try {
       			url = new URL(stringUrl);
        	} catch (MalformedURLException exception) {
                Log.e("MainActivity", "Error with creating URL", exception);
                return null;
        	}
            
        	    return url;
            ```
        6. After that, we declare the request method, `private String makeHttpRequest(URL url) throws IOException {}` Try to ***discuss*** everyline of code with your friend, and make sure you understand, Google it if needed.
           
           ```java
            String jsonResponse = "";
            HttpURLConnection urlConnection = null;
            InputStream inputStream = null;

            try {
                urlConnection = (HttpURLConnection) url.openConnection();
                urlConnection.setRequestMethod("GET");
                urlConnection.setReadTimeout(10000 /* milliseconds */);
                urlConnection.setConnectTimeout(15000 /* milliseconds */);
                urlConnection.connect();
                if (urlConnection.getResponseCode() == 200) {
                    inputStream = urlConnection.getInputStream();
                    jsonResponse = readFromStream(inputStream); // read the response data and make it as a single string
                }
                else {
                    Log.e("MainActivity", "Error response code: " + urlConnection.getResponseCode());
                }
            } catch (IOException e) {
                Log.e("MainActivity", "Problem retrieving the book JSON results.", e);
            } finally {
                if (urlConnection != null) {
                    urlConnection.disconnect();
                }
                if (inputStream != null) {
                    // function must handle java.io.IOException here
                    inputStream.close();
                }
            }

            return jsonResponse;
           ```
           
        7. As listed in `makeHttpRequest()`, we have `readFromStream()`, so we declare anything inside it here, we build a string from everyline of response
        	```java
            StringBuilder output = new StringBuilder();

            if (inputStream != null) {
                InputStreamReader inputStreamReader = new InputStreamReader(inputStream, Charset.forName("UTF-8"));
                BufferedReader reader = new BufferedReader(inputStreamReader);
                String line = reader.readLine();
                while (line != null) {
                    output.append(line);
                    line = reader.readLine();
                }
            }

            return output.toString();
            ```
        8. As listed in `doInBackground`, we have `extractBookInfoFromJson(String bookJSON)`, so we declare anything inside it here,
        	1. First we check the input parameter, is it empty or not
				```java
            	if(TextUtils.isEmpty(bookJSON)) {
            	   return null;
        		}
            	```
            2.  Then since the `extractBookInfoFromJson()` will return a `ArrayList<Book>`, we declare a new book variable `ArrayList<Book> books = new ArrayList<Book>();`
            3.  Next, we try to parse the String input of this method, the `bookJSON` inside a `try/catch`. Now we `try` to parse `bookJSON`

				**First**, cast it into `JSONObject` and check  the `"totalItems"` key inside that `JSON`. If the item is zero, then there is no book matched our keyword.
            	```java
            	JSONObject baseJsonResponse = new JSONObject(bookJSON);
                if(baseJsonResponse.getInt("totalItems") == 0) {
                    runOnUiThread(new Runnable() {
                        public void run() {
                            Toast.makeText(MainActivity.this, "Result not found.", Toast.LENGTH_SHORT).show();
                        }
                    });

                    return null;
                }
            	```
                
                **Second**, we cast the `JSONObject` into a `JSONArray`. ***Please Google it if you don't know what's the different between the two*** .
       		`JSONArray itemArray = baseJsonResponse.getJSONArray("items");`
            
            	**Third**, using `for/loop`, we iterate through the `itemArray` to get the needed information. Before that, try to see the response data using browser, to see the `key` name. For example, we have *"makers"* as the keyword, so the final URL will be like, `"https://www.googleapis.com/books/v1/volumes?q=makers"`. Open that on your browser. If you can't see the pattern, try to `beautify` the JSON, using [this](http://codebeautify.org/jsonviewer), or [this](https://jsonformatter.curiousconcept.com/)

				**Fourth**, inside a `for/loop`, like this `for (int i = 0; i< itemArray.length(); i++) {}`, we will parse the `JSON` to get specific information
                ```java
                // Extract out the cuurent item (which is a book)
                JSONObject cuurentItem = itemArray.getJSONObject(i);
                JSONObject bookInfo = cuurentItem.getJSONObject("volumeInfo");
                ```
                
                ```java
                // Get the book title
                String title = bookInfo.getString("title");
                ```
                
                ```java
                // Get the list of book's author(s)
                String [] authors = new String[]{};
                JSONArray authorJsonArray = bookInfo.optJSONArray("authors");
                if(authorJsonArray!= null) {
                    ArrayList<String> authorList = new ArrayList<String>();
                    for (int j = 0; j < authorJsonArray.length(); j++) {
                        authorList.add(authorJsonArray.get(j).toString());
                    }
                    authors = authorList.toArray(new String[authorList.size()]);
                }
                ```
                
                ```java
                // Get the book's description
                String description = "";
                if(bookInfo.optString("description")!=null)
                    description = bookInfo.optString("description");
                ```
                
                ```java
                // (Optional) get the link to the book
                String infoLink = "";
                if(bookInfo.optString("infoLink")!=null)
                    infoLink = bookInfo.optString("infoLink");
                ```
                
                ```java
                // Add book information info into books array
                books.add(new Book(title, authors, description, infoLink));
                ```
                
                **Sixth**, inside the `catch`, we catch the possible error
                ```java
                catch (JSONException e) {
                    Log.e("MainActivity", "Problem parsing the book JSON results", e);
            	}
                ```
                
                **Finally**, return the `books` since this method required you to return an `ArrayList<Book>` and the `books`'s type is `ArrayList<Book>`
                ```java
                return books;
                ```
                
                
## Congrats!
