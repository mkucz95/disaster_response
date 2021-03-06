# Disaster Response Pipeline Project
## Project Description
In this project the goal was to look at messages (ie from social media) at a time and location of disaster and look at the possibility of classifying these messages in a number of different categories including whether the message is related to the disaster, and, if so, in what way. This would potentially enable better emergency services responses.

### ETL Pipeline
I first built an ETL pipeline that streamlined the Extract-Transform-Load steps of data processing, and could be extended to take data from numerous sources, or be used to continually update the existing database of messages. In the current version, the data comes from two separate csv files, one with the messages and their genre (direct, news, or social). The other file contains the categories the message falls into (related to a disaster and in what way). This involved creating a feature for each category and converting strings that contained category and value information to only depict the numerical value for each category. The final dataset contains information on more than 26,000 messages.

### Machine Learning Pipeline
Secondly, I looked into building the best possible Machine Learning algorithm to classify the messages. This involved using GridSearch optimization to find the best parameters of the ML algorithm. It turned out that the AdaBoost Ensemble classifier had the best performance. The next step was to build a Machine Learning pipeline using the AdaBoost classifier, and then saving this model to a file for future use. The benefits of using the pipeline is that I could combine various features - not only TFIDF and part of speech tagging - to improve the performance of the model. 

I created custom classes: `NumericalExtractor`, `StartingNounExtractor`, and `MessageLengthExtractor`. These extractors allowed the ML algorithm to consider whether the message contains numbers, whether it starts with a noun and how long the messages during training and classification. Furthermore, the pipeline enabled the use of a multioutput classifier. This means we could classify numerous features concurrently.

### Dataset Imbalance
There are numerous categories such as `security`, `clothing`, `aid_centers`, `missing_people`, `hospitals`, `fire`, and `child_alone` that have less than 500 true occurences. This presents a challenge for the ML algorithm training as it does not have many chances to understand how it should classify messages that fall under those categories. This means that the performance of the algorithm in relation to those categories will probably not translate well to new data, unless we have more examples to train the algorithm on.

To put this algorithm to action, I created a dashboard utilizing Flask. There are data visualizations giving an idea about the distribution of messages in the dataset, as well as the ability to classify a custom message from the dashboard.

## Included Files
`/data`
	- `DisasterResponse.db`: the sqlite3 databse file that is output by ETL pipeline
    - `process_data.py`: the python script that runs ETL file
    
`/models`
	- `ada_classifier.pkl`: the pickled classifier that was trained and optimized for classifying the dataset messages
    - `train_classifier.py`: the script that builds, trains, and saves the classifier.
    
`/app`
	- `run.py`: the script that runs the flask app
    - `/templates`
    	- `go.html`
        - `master.html`: the default page for the app
    - `/static`
    	-`/styles`
		- `style.css`: custom styling for dashboard page
        
## Instructions:
### Command Line & Local Host
1. Run the following commands in the project's root directory to set up your database and model.

    - To run ETL pipeline that cleans data and stores in database
        `python data/process_data.py data/disaster_messages.csv data/disaster_categories.csv data/DisasterResponse.db`
    - To run ML pipeline that trains classifier and saves
        `python models/train_classifier.py data/DisasterResponse.db models/classifier.pkl`

2. Run the following command in the app's directory to run your web app.
    `python run.py`

3. Go to http://0.0.0.0:3001/

### Online Deployment
Working on next steps to deploy online
