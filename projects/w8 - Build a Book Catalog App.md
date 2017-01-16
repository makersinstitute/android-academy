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
		3.  Add `ClickListener` for search button. First, show the loading view, then call the request method. We use the `AsyncTask` here.
			```java
            BookAsyncTask task = new BookAsyncTask();
            task.execute();
            ```
     4. 

## Congrats!
