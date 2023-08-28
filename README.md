Practical Application 2:

In this data set I was working on data from used car dealerships which consisted of features such as condition, color, odometer number, number of cylinders, etc.

My job is to generate an accurate model to predict prices as well as determine what features lead to higher prices which I can use to tell future used car dealerships so that they can try and maximize their profits.

Step 1: Understanding the Data

I checked the features of the data to see if there was anything I could catch on to without the need of processing it. I immeditely noticed that columns such as ID and VIN were not of use to me since they are all unique values for each row and would not provide any help in making a model or comparing features to each other, so I decided to drop those columns.

I made a pairplot of the numerical features to see if there were any patterns in the data, but I couldn't see anything conclusive about the odometer and year features related to the price of the car.

Step 2: Preprocessing

I wanted to take 2 approaches with the preprocessing: The data set has a lot of rows, upwards of 400,000; so I decided to have 2 cases, one where I just drop all the rows with ANY NaN values, and a second where I do much more detailed preprocessing to keep as many rows/columns as possible.

Case 1: Dropping all rows with any NaN values

I noticed that this data set has a lot of entries and as such has a lot of columns with missing entries. I decided to make an inital Linear Regression model in order to make a permutation importance data frame to see what I can do with the NaN values in certain columns since I want to try and preserve as much data as possible.

After this I took all the categorical columns and one hot encoded them, then I took the numerical columns excluding the price (since this is the target variable) and ran them through StandardScaler.

Step 3: Building Models

I saw that the top 6 features of importance were: region, fuel, year, state, type, and odometer, I wanted to save these features and keep them in mind for when I ran different models later. I used these features in another linear regression model, where the features named were the ones used for training and testing, then I captured the mse and mae for this model.

Next I wanted to try a model using SequentialFeatureSelector to see if the features that it picked would make a better model than my current Linear Regression model. I rean this with a number of features being 4 (I found that anything after 5 for this model was taking a bit too long), this outputted the features: 'cylinders_8 cylinders', 'fuel_diesel', 'drive_fwd', and 'year', then I plugged these features into another Linear Regression model and captured the mse and mae for it.

The last model I wanted to try for this data set was using RFE and Ridge regression to see if I was possibly overfitting data and could improve the mse and mae for the validation set. I made this model with 5 features selected and this outputed the features: 'manufacturer_aston-martin', 'manufacturer_ferrari', 'manufacturer_porsche', 'manufacturer_tesla', and 'fuel_diesel'. I took these features and ran them through a Ridge model and got the mse and mae for it.

My findings from these models was that the original set with the features from permutation importance definitely performed the best in both the training and validation data sets. This tells me that the features used from the permutation importance and the first regression model yield the best output.

Case 2: More preprocessing on data set

In this case I wanted to take my result from the previous permutation importance and use it to better clean the data on the data set so that I could have more rows to work with. From this I filled in the features that were in the middle of the permutation importance list with the mode of those respective columns, fully dropped columns that I thought would be too troublesome to deal with (such as the model feature, I found that one hot encoding this and running it through models took too long since there were upwards of 5,000 individual models which would make processing of the data in the model take much longer). I was left with around 390k rows of data.

From here, I ran a simple Linear Regression model with every feature in the cleaned data set and got the mse and mae.

Next I ran another model with SequentialFeatureSelector with Lasso and with 5 features selected, the features that this model gave me were: 'manufacturer_aston-martin', 'manufacturer_ferrari', 'manufacturer_porsche', 'manufacturer_tesla', and 'fuel_diesel'. Next I used these features and ran the training and test data through another regression model with only these features and got the mse and mae for it.

I found that the mse and mae for the model with SequentialFeatureSelection actually performed better on both the training and validation data which surprised me, I would've thought that more data and features would yield a better model, but this leads me to believe that there could still be some noise or features that aren't necessary in the price computation from the model, which if I were to continue with this project I would try to find.

Step 4: Conclusion

Overall, the features that attribute the most to the price of a car are: region, condition, odometer, year, type, fuel, and state. Specifically values for these features such as white paint, diesel fuel, excellent condition (or fair condition if looking at the mean and not the sum of the price values) and specific manufactureres like ferrari and rover. If I were giving advice to a new used car dealership, I would tell them to definitely prioritize these features because based on the model this would bring in the most revenue and it's what customers on average are buying the most of.
