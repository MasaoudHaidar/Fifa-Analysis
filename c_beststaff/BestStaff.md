# Which Club has the Best Staff
- Study player data from Division 1 European League* players from the last 5 Years. Analyze changes in player stats and value. Rank the clubs according to best increase in statistics of a player.
- Graphically represent the scores for the test set.

# Instructions :
- Sofifa Id, URL, Wage, Salary, Name, Real face, URL variables cannot be used during the prediction or learning.
- Assign a score to all clubs out of 100, and represent it in an appropriate visualization. With 100 being the highest
- For the test set, your model will be scored using MSE against the average of all models of the class. (Score = MSE( Your Values , Average of all models values))

# Test Set:
The test set is as follows for this problem statement (Division 1 European League):
League =
- Premier League - English Premier League
- Bundesliga German 1. Bundesliga
- Ligue 1 French Ligue 1
- La Liga Spain Primera Division
- Serie A Italian Serie A

# EDA Activities:
We only take into account players that stayed at the club in consecutive years. Then, find the difference (improvement or decline) in their response variable (such as overall rating) through years. Response variable example: If our chosen variable is OVR, then our response variable would be the average change of OVR in a club from a year to another, for players who stayed at the club.

## Data Cleanup

The purpose of the cells below is to make sure that we drop the columns that have been identified as not to be used.

In addition to the columns indicated in the statement, the team also feels that the clubs staff capabilities do not depend on other attributes like:
1. Player Height
1. Player Weight
1. Player Nationality

So these columns have been also identified as to be dropped.

## Data Analysis

For the purpose of Data Analysis, identifying columns which are numerical would simplify the quantitative analysis of the stafs capabilities. In order to identify those columns and run some preliminary analysis like min values max values, identifying the columns which are numeric.

### Step 1: Identify Numerical Columns

#### Inference from the step:
The list above indicates that there are about 24 columns which are numerical and can be leveraged for the sake of quantitative analysis. 
However further investigation of these numerical columns indicate that some of these columns could be added to the cleanup of columns as they would not really reflect the staffs capabilities to promote talent.

#### Inference Action:
Add the columns to the list of columns to be cleaned up and remove the columns.

Columns identified:
1. value_eur
1. release_clause_eur
1. team_jersey_number
1. contract_valid_until
1. nation_jersey_number

### Step 2: Analyze the Numerical Columns

In order to check the quality of data available in those numerical columns, analyzing a couple of years to see the type and quality of data. 

#### Inferences:
1. The Describe on the multiple years does indicate that the mental_composure is not a column that we can rely on as the values are not captured in the earlier years and further investigation indicated that the data has a format which is a split value (e.g. and hence can be excluded 90+3)
1. International reputation is also a Categorical variable with values 1 to 5.
1. Weak Foot is a categorical value too with values 1 to 5.
1. Numerical columns 'gk_speed', 'gk_handling', 'gk_kicking', 'gk_positioning', 'gk_diving' have very few values. On inspecting the players associated with those values, it was identified that these are goal keepers and attributes for goal keepers. As the question is about effectiveness of the entire staff, the approach was to drop these columns as well both for lack of data and to avoid focus on skill development for goal keepers.

#### Inference Action
1. Remove the mentality_composure value from the columns. 
2. From domain knowledge perspective, weak foot can be eliminated as other factors would reflect if the staff has improved the weak foot score of the individual.
3. International reputation is also being dropped because we are not measuring PR teams capabilities but the teams staff.
4. Remove the gk_* values from the columns list.  
5. Also as the Goal Keepers are missing the information of other attributes, we might not be able to get metrics on the staffs work on the goal keepers. 

### Skills Columns

The skills columns in the dataframe indicate the different skills with a numerical value for the Skills. The team thinks that the skills are an important part of the development of every player. The player skills once improved will contribute to the overall improvement of the player and hence its overall rating. 

#### Observations:
1. Non Numeric columns: 
The columns for skills are multiple and are non numeric. Infact the columns have values with + and - signs indicating that the columns have data which indicates positive and negative traits.

#### Observation Action:

1. The decision is to exclude the columns for the first set of determination and keep only the numeric columns in the list. 
2. Build a data frame for every year with only these numeric values in addition to columns which identify the player like name, club.

#### Numerical Only Dataset

Creating a new Dataset with only numerical columns:
0.          age
1.      overall
2.    potential
3.  skill_moves
4.         pace
5.     shooting
6.      passing
7.    dribbling
8.    defending
9.       physic
10. short_name
11. club

### Year Over Year Comparison

#### Approach
Since the purpose of the exercise is to identify the effectiveness of the staff, the approach is to identify the score differences of the players year over year. 

We are going to join data year over year and see the score differences. The approach is to create a dataframe that would have the name of the player the club and the difference in the rating of the numeric column for the player.

#### Decisions:
1. Initially the team considered only players who stayed at the club to get a guage of the players skill. However given that not many players were staying at the club, the decision was to not to enforce the stay at club metric. The credit for the increase is being assigned to the club in the earlier year.
1. The score difference will be calculated for every numeric parameter including the values which are categorical.

### Score Difference Matrix

The year over year dataframe has all the score changes for the Club for every individual player that has played for the club.

#### Approach 1
Find the number of players who have a positive change for every metric. This would mean that we create a histogram and find how many players have a positive change. We are going to only count players who have atleast one standard deviation change of positive change in rating for every metric. That will give us indication of how important is this metric in staffs contribution.

#### Approach 1 Findings:

* From the graphs above it is clear that several teams are fiding improvements in players year over year. Of the parameters which were considered:
1. diff_overall
1. diff_potential
1. diff_skill_moves
1. diff_pace
1. diff_shooting
1. diff_passing
1. diff_dribbling
1. diff_defending
1. diff_physic 
    all the parameters seems to have the same weight but looks like defending value seems to carry a different weight. 
* Also printing the top 10 clubs which have the most players for each individual trait is not helping. A comprehensive groupoing of all the clubs and calculating the top 100 clubs order might be a consideration.

#### Approach 1 Findings Action
Keep all the parameters in a similarly weighted fashion in a formula to compute a staff score. Consider giving Defense a slightly higher weight.

#### Approach 1 Extension:

By checking the top 10 clubs and the average improvement in the values of the clubs, we observe that the values of the overall score do seem to have an aggregate value for the scores of the individual components and could be used to calculate a score for the best staff score.