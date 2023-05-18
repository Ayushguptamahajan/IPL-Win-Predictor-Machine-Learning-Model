# IPL-Win-Predictor-Machine-Learning-Model
IPL Match win VS loss probability predictor

### **The summary of the steps executed in ML model are:**

**1. Data collection and overview of the dataframe:**
- The data was extracted from csv file downloaded from kaggle (matches.csv and delievry.csv).
- There were 756 matches played from the year 2008 - 2019 as depicted by matches dataframe.
- There were 179078 rows and 21 columns depicting the various details of match played in IPL ballwise.
- Intution of the dataframes was gained by calling the first five and last five rows of the given dataframe.
- There were ~1.4% values (feature city) and the same was droped by calling dropna() function.

Since we are going to find the probability of winng and lossing after each over similar to various app available in market.
we will be fetching the below feature from above two available dataframe:

- batting_team 
- bowling_team 
- city (where corresponding match was played)
- runs_lefts (pending runs to win the match after that corresponding ball during the second innings)
- balls_left (pending balls that can be played after that corresponding ball during the second innings) 
- wickets_left (No of wickets left of the batting team in the second innings at that state of time)
- total_runs_x (Represent the target runs to secure i.e first inning score)
- ccr (Current run rate)
- rrr (required run rate)
- result (whether the match was win by the current team at the corresponding ball)

Above feature was obtained with intuitive implementation of various libraries and new features was constructed
Based upon above our machine learning program will be trained to predict the probability of the match winner team.


**2. Feature Engineering:**

Below feature was constructed to reach our final dataframe:
- batting_team : Feature already available in the delivery dataframe.
- bowling_team : Feature already available in the delivery dataframe.
- city : Feature already available in the matches dataframe.
- runs_lefts : calculated based upon the target set in the first inning of each match (Merge, groupby etc function was used) 
- balls_left : was fetched by the formula (126 - delivery_df['over']*6 - delivery_df['ball']) from the merged delivery dataframe 
- wickets_left : was fetched by the formula (10- delivery_df.groupby('match_id').cumsum()['player_dismissed']) from the merged delivery
- total_runs_x : fetched from the target set during the first inning with the help of groupby function.
- ccr (Current run rate) : calculated from merged delivery datafram
- rrr (required run rate) : calculated from merged delivery datafram
- result : Function created. if the balling team wins than it return 1 else 0.

At last final_df was created having above 10 features 

Note: 
- we need to eliminate the rows where dlapplied was true as the same is not the parameter for prediction
- we also need to remove NaN values coming in the rrr feature
- In view of stat obtained for the final_df dataframe, there inf and inf values in the rrr feature. The same occured due to presence of 0 in the ball_left feature. So all the 0 values in ball_left feature was dropped out.


In order to create randomness in the dataframe sampling was done.

After obtaining the final_df, One hot encoding was implementated to batting_team,bowling_team and city which returned an array of 48 columns.
Thereafter scaling was implemented to all the 48 columns obtained after encoding.

In order to ease out the process of transformation of columns, columntransformer fucntion was called and finally a Pipe named pipeline was called.

**3. Exploratory Data Analysis:**

- Dist and QQ plot was ploted for numerical columns which depicted none of the numerical columns are normally distributed. Reason being presence of outliers.
- Boxplot was also plotted for numerical columns.


**5. Model Building:**

- Train test split was performed with test_size as 10% and random state of 1.
- Further **two** model namely **Logistic Regression** and **Random forest calssifier** was fitted for the splitted data set and various accuracy_score/prescission score was fetched.


Even though the accuracy_score of Random forest classifier comes out to be more but the value is very high 99%.
We will use Logistic regression because the sigmoid function in the Logistic regression give good result during probability problem.

Finally a pipe named pipeline1 was constructed which includes the Model Logistic Regression and above Columntransformers.

**5. Conclusion:**

**IMPORTANT IMPLEMANTATION:**

As the end goal was to check the probability of win and lose at end of each over, self has created one function named match_progression provide the target (Runs during first inning) and a dataframe. The dataframe obtained from the the said function will provide the probability of lose and win after each over for a particular match. Other details like runs made in that over and wicket gone in that over will also be depicting in that dataframe.

Graph of various parameter of match condition was displayed to know the trend of winning and losing percent after each over. 
