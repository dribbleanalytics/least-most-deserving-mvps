# METHODOLOGY: Finding the least and most deserving MVPs of the last decade

If the .ipynb titled "master-least-most-deserving-mvps" does not open, try opening the "master-least-most-deserving-mvps-NO-OUTPUTS" file. The original has all the outputs saved, making it a large file which GitHub sometimes can't open.

[Link to blog post.](https://dribbleanalytics.blogspot.com/2018/08/least-most-deserving-mvps.html
)

## Data collection

First, I created a database of all the players who were top 10 in MVP voting since the 1979-1980 season. This was chosen as the cutoff because the 3 point line was introduced that year.

The following stats were recorded for these 388 players:

| Counting stats | Advanced stats| MVP votes | Team stats |
| ------------- | ------------- | ------------- | -------- |
| G | WS | MVP votes won | Wins |
| MPG | WS/48 | Maximum MVP votes | Overall seed |
| PTS/G | VORP | Share of MVP votes* |  |
| TRB/G | BPM | |
| AST/G | | |
| STL/G | | |
| BLK/G | | |
| FG% | | |
| 3P% | | |
| FT% | | |

*Vote share = percentage of maximum MVP votes. So, all the players' vote share does not add up to 1 for all players. Only a player who receives every first place vote will have a vote share of 1 (Curry 2016).

For lockout years - 1998-1999 and 2011-2012 - I scaled up the win shares, VORP, and team wins according to the number of games in the lockout year; 1998-1999 was scaled up from 50 to 82, and 2011-2012 was scaled up from 66 to 82.

All data was taken from [Basketball Reference](http://basketball-reference.com/).

## Model creation

Using scikit-learn, I created four models: a support vector regression (SVM), a random forest regression (RF), a k-nearest neighbors regression (k-NN), and a multi-layer perceptron regression (or deep neural network, DNN). Each model used scikit-learn's train test split function with a test size of 25%.

The models were created to take all of the above inputs except for WS/48. This is because WS/48 can be linearly predicted by MPG and WS, so having all three factors in a model create collinearity.

Each model's mean squared error and variance score (r-squared) was measured to find the most accurate regression. Each model's cross-validation score for r-squared was also measured to help determine the most accurate model. Note that a higher rsquared and explained variance indicates a more accurate regression, but a lower mean squared error indicates a more accurate regression.

Along with these basic goodness-of-fit tests, I performed a standardized residuals test, Durbin-Watson test, and Shapiro-Wilk test. In addition to these test, I created Q-Q plots and learning curves.

## Finding the least and most deserving MVPs

All the models predict a player's vote share. Vote share is predicted instead of total votes because the maximum number of MVP votes increased over time, so using vote share standardizes all the results.

The player with the highest predicted vote share is who the models predict should have won MVP. The player with the second highest predicted vote share should have come second in voting, and so on.

The models were trained and tested on MVP data from 1979-2007, and then made predictions for the last decade of MVPs (2008-2018). The predicted vote shares were plotted against the actual vote shares to clarify the least and most deserving MVPs.
