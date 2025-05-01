# Machine Learning - Basics 1

## Pandas

```
import pandas as pd

file_path = '../input/whatevert/somefile.csc'
thedata = pd.read_csv(file_path) 
thedata.describe()
thedata.columns
```

## Models

Drop data that may have missing values in the columns
```
thedata = thedata.dropna(axis=0)
```
We can choose the column we want to predict as y
```
y = thedata.Price
```

we can also choose our features, or what columns we will use to predict the price, those would be like x1, x2...
```
thefeatures = ['col1', 'col2', 'col3', 'col4', 'col5']
X = thedata[thefeatures]
```

we can check it out

```
X.describe()
X.head() #top rows
```

We can build a model, we usually use some data to build the model and some data to test the model
```
from sklearn.tree import DecisionTreeRegressor

# Define model. Specify a number for random_state to ensure same results each run
themodel = DecisionTreeRegressor(random_state=1)

# Fit model - Capture patterns from provided data.
themodel.fit(X, y)
```

When we are predicting, the error will be actual−predicted

Once we have the model, we can find the error
```
from sklearn.metrics import mean_absolute_error

predicted_home_prices = themodel.predict(X)
mean_absolute_error(y, predicted_home_prices)
```
But we know that we have to use some data for the model and for the testing of the model.

```
from sklearn.model_selection import train_test_split

# split data into training and validation data, for both features and target
# The split is based on a random number generator. Supplying a numeric value to
# the random_state argument guarantees we get the same split every time we
# run this script.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)
# Define model
themodel = DecisionTreeRegressor()
# Fit model
themodel.fit(train_X, train_y)

# get predicted prices on validation data
val_predictions = themodel.predict(val_X)
print(mean_absolute_error(val_y, val_predictions))
```

The more we split up the data (deeper decision tree) the better the prediction will be.  Though you don't want to overdue it.

```
from sklearn.metrics import mean_absolute_error
from sklearn.tree import DecisionTreeRegressor

def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
    model = DecisionTreeRegressor(max_leaf_nodes=max_leaf_nodes, random_state=0)
    model.fit(train_X, train_y)
    preds_val = model.predict(val_X)
    mae = mean_absolute_error(val_y, preds_val)
    return(mae)

# compare MAE with differing values of max_leaf_nodes
for max_leaf_nodes in [5, 50, 500, 5000]:
    my_mae = get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y)
    print("Max leaf nodes: %d  \t\t Mean Absolute Error:  %d" %(max_leaf_nodes, my_mae))
```

