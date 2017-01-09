![Makers Institute](../images/logo-makersinstitute.png)
-

# Week 7: Student Report App

## Project Overview
This project is a chance for you to combine and practice everything you learned so far. You will be making a student report app.

The goal is to design and create the structure of a Student Report App which would allow a student to store a student's grades for a particular subject.

## Why this project?
In the most recent portion of the class, you learned about many things that related to object-oriented program. It is vital to be able to think about how to design objects which interact with each other and model real-world concepts.

## What will I learn?
This project is about combining various ideas and skills we've been practicing throughout the class. They include:

- Designing an object class
- Using `ListView`
- Create a Tabbed Activity using `ViewPager`
- Using `StartActivityForResult`
- Taking picture using camera
- `If/else` statement.

## Build Your Project

Here are some prefered order to build the app. If you are feeling stuck or confused, try to ask the class Instructor, class assistant, your friend, or our best friend, Google Search Engine.

1. Create a new project
2. Create a `Score` model

	```java
	public class Score {

	    String subjectName;
	    Double subjectScore;

	    public Score(String subjectName, Double subjectScore) {
		this.subjectName = subjectName;
		this.subjectScore = subjectScore;
	    }

	    public String getSubjectName() {
		return subjectName;
	    }

	    public void setSubjectName(String subjectName) {
		this.subjectName = subjectName;
	    }

	    public Double getSubjectScore() {
		return subjectScore;
	    }

	    public void setSubjectScore(Double subjectScore) {
		this.subjectScore = subjectScore;
	    }
	}
	```
3. Create a `ScoreAdapter` also the custom item view containing two text view side by side for showing you report details.
	```java
	public class ScoreAdapter extends ArrayAdapter<Score> {

	    public ScoreAdapter(Context context, List<Score> scoreList) {
		    super(context, R.layout.score_item, scoreList);
	    }

	    public static class ViewHolder{
		TextView subjectName;
		TextView subjectScore;
	    }

	    @NonNull
	    @Override
	    public View getView(int position, View convertView, ViewGroup parent) {
		//get the data item for this position
		Score score = getItem(position);
		ViewHolder viewHolder;

		// Check if an existing view is being reused, otherwise inflate the view
		if(convertView == null){

		    // if there is no view created
		    // create new view
		    convertView = LayoutInflater.from(getContext()).inflate(R.layout.score_item,null);
		    viewHolder = new ViewHolder();
		    viewHolder.subjectName = (TextView)convertView.findViewById(R.id.text_view_subject_name);
		    viewHolder.subjectScore = (TextView)convertView.findViewById(R.id.text_view_subject_score);
		    convertView.setTag(viewHolder);
		} else {
		    viewHolder = (ViewHolder)convertView.getTag();
		}

		//set Text
		viewHolder.subjectName.setText(score.getSubjectName());
		viewHolder.subjectScore.setText(score.getSubjectScore().toString());

		//retun view of item
		return convertView;
	    }

	}
	```

4. Build a `Tabbed Activity` using `ViewPager` and containing two fragments.
5. Layout the first fragment, which is your profile.
6. Add taking picture function
7. Hardcode your name and your institution/occupation
8. Layout the second fragment, which is your report summary.
9. Add a `Button` to view your report details
	- Use `startActivity` on the button's `ClickListener`
	- Report details contains a `ListView` of your score
10. Inside report details, add another button, prefered `FloatingActionButton`, for adding a data to report details' `ListView`.
	- The `FloatingActionButton`'s `ClickListener` using `StartActivityForResult`
	- The `StartActivityForResult` will open a new activity for adding data to the `ListView`
11. Create a new `Activity` that contains two `EditText` and a `Button`
	- First `EditText` is for Subject's Name, and the second one for Subject's Score
	- On `Submit` button's `ClickListener`, create a result `Intent` with the same `requestCode` as the `StartActivityForResult`
	- Don't forget to add `finish();` to destroy the activity.
	- *Optional*: make sure to filter number only for score's input.
12. Back to the report details, override the `OnActivityResult` and get the `Intent` data.
	- Make sure you check the `requestCode` and the `resultCode`
	- Add the data to array list

	```java
	ArrayList<Score> scoreArrayList; // global variable

	// some code...

	// inside onCreate()
	scoreArrayList = new ArrayList<Score>();

	// inside onActivityResult()
	addedSubject = data.getStringExtra("subjectName");
	addedScore = data.getDoubleExtra("subjectScore", 0.00);

	scoreArrayList.add(new Score(addedSubject, addedScore));
	scoreAdapter.notifyDataSetChanged(); // to refresh the ListView that there is a new data
	```
	
13. When the ListView is showing every input you just made, create a function to count the average.
	- Save the average number into SharedPreference
	```java
	public void countAverage() {

		Double sumScore = 0.0;
		averageScore = 0.0;
		for (int i = 0; i < scoreArrayList.size(); i++) {
		    sumScore += scoreArrayList.get(i).getSubjectScore();
		}

		averageScore = sumScore / (double) scoreArrayList.size();

		editor.putLong("averageScore", Double.doubleToLongBits(averageScore)); // because SharedPreference can't hold a Double, so we convert it into Long first
		editor.apply();

	    }
	```

14. Back to the second fragment, the report summary, get the average number from SharedPreference.

	```java
	public void getAverageScore() {

		averageScore = Double.longBitsToDouble(sp.getLong("averageScore", defaultValue));
		averageScoreText.setText(averageScore.toString());

	    }
	```

15. Now, process the average number, using if/else statement. If the score is below something, the description will be something, and else.

	```java
	public void setDescriptionText() {

		if (averageScore < 50) {

		    scoreDescriptionText.setText("E");

		} else if ... {

		}

	    }
	```

*NB*: If you see that the `ListView` data is missing on every button click, it's ok, since we are not saving the `ListView` data.

## Congrats!
