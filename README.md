# Indoor-Positioning-Via-Wifi-Fingerprinting
Predicting people's location (Building, Floor and Coordinates) at Jaume I University from WAPs signal information using classification and regression machine learning algorithms (K-NN, Gradient boosting, and Random Forest)

# About the datasets
Source of the datasets: http://archive.ics.uci.edu/ml/datasets/UJIIndoorLoc

Two datasets, training and validation, contain information about 520 WAPs signal strengths, three buildings and their floors, coordinates of users who logged in, space where the users logged in and the relative position (inside or outside the room), user ID, phone ID, and timestamp. 
The data was collected at Jaume I University.
More information about the dataset can be found on the source link provided above. 

# Objective
My objective is to build models that predict the location (building, floor and coordinates) from the WAPs signal strengths of a user who connects to the internet at Jaume I University. 

# Procedure
I have cleaned the data, put the attributes into proper data types, subsetted dataset for each buiding, examined each column, and normalized WAPs rows in order to build models. I have used different algorithms (KNN, Random Forest, and Gradient Boosting) to predict the building, floor and coordinates of a logged in user. 

The first step was to remove columns and rows where WAP signal strength has only one value, meaning that the WAP was not detected. Also, the columns were converted into appropriate data types. 
Original dataset used WAP signal range from -104 to 0 (-104 being the lowest signal and 0 the highest), and value 100 when WAP was not detected. I have changed the values so that 0 represents the value when WAP was not detected and the highest signal is 104.  

The figure below shows the distribution of WAPs singal strength for each dataset. 
![distribution together](https://user-images.githubusercontent.com/32273216/35473323-f09f8c72-037e-11e8-82f6-2060a360c51b.jpg)

The following figure shows the distribution of WAPs signal strength on the same histogram in order to see the comparison clearly.

![03 - waps distribution training and test](https://user-images.githubusercontent.com/32273216/35473334-36cf6f5a-037f-11e8-922f-c559dd2756bc.png)

Number of WAPs detected per building for training and validation set can be seen in the figure below.

![number of waps both buildings](https://user-images.githubusercontent.com/32273216/35473388-232fe15e-0380-11e8-9242-60fddb9eaa27.jpg)

The coordinates in the dataset are given in World Geodetic System form as Longitude and Latitude. I have converted the to absolute values starting from 0. 

Log In locations of users from trainining and validation sets as well as a google map screenshot of the buildings of Jaume I University are shown in the figure below.

![university plot and google map](https://user-images.githubusercontent.com/32273216/35731801-5dd3c500-0817-11e8-87db-f3becb150b52.jpg)

The following figure shows the log in locations of users at each floor in Building 1 from the training set. 

![07 - log in locations building 1](https://user-images.githubusercontent.com/32273216/35473449-3b879598-0381-11e8-9951-5ae6b8dde1c9.png)

I have checked the locations were signal were in good, medium and bad range. For example, the following figure shows the locations at which users had signal higher than 60 (out of 104).

![good signal longlat](https://user-images.githubusercontent.com/32273216/35473500-d58e332c-0381-11e8-82e7-f1b008340055.png)

It can be seen that least amount of high WAP signals was recorded in the middle building. Later in the modeling process this will cause lower accuracy of location predictions for this building. 

# Results (Machine Learning Models)

I started with building the model to predict the building at which user had connected to the internet. 
The first model had low accuracy so I decided to normalize WAPs rows put them in range from 0 to 1) and it drastically improved the model's performance. Also, different WAPs in test and validation set are not detected. It was important to remove these columns from both sets and leave only the columns that intersect in both datasets. 
Another thing that further improved the performance of the model was choosing the threshold for WAPs signal strength. With normalized data, I have found that the best performance is achieved when building models only with rows where the average value is above 0.6 (excluding non-detected WAPS)

## Building prediction (Classification with KNN model)

With normalized WAP rows, the achieved accuracy is 100% for buildings' prediciton on validation set with KNN model. 
The performance and confusion matrix of the model can be seen below:

**Accuracy = 1**

**Kappa = 1**

**Confusion Matrix:**

                         |   0   |    1   |   2   
                         ------------------------
                      0  |  536  |    0   |   0   
                         ------------------------
                      1  |   0   |  307   |   0   
                         ------------------------
                      2  |   0   |   0    |  268  
                    

## Floor prediction (Classification with KNN, Random Forest, and Gradient Boosting models)

The best performance for all three  building were achieved with Random Forest algorithm and the results are given in the following section. However, it should be noted that KNN and Gradient Boosting models are significantly faster and in floor predictions for Building 1 and Building 3 the difference in performance (about 1%) might be negligible.

### Building 1 Floor prediction

The best performance was achieved with Random Forest Algorithm. The performance and confusion matrix are shown below:

**Accuracy = 0.9757**

**Kappa = 0.9657**

**Confusion matrix:**

                            |   0   |    1   |   2   |  3
                            --------------------------------
                         0  |  75   |   2    |   0   |   0
                            --------------------------------
                         1  |   3   |  204   |   3   |   0
                            --------------------------------
                         2  |   0   |   2    |  162  |   3
                            --------------------------------                       
                         3  |   0   |   0    |   0   |   82


### Building 2 Floor prediction

The floor prediction for this building has the lowest performance as it was mentioned. The best achieved accuracy is 89.9%.
The best performance was achieved with Random Forest Algorithm. The performance and confusion matrix are shown below:

**Accuracy = 0.8990228**

**Kappa = 0.8520298**

**Confusion Matrix:**

                           |   0   |    1   |   2   |  3
                           --------------------------------
                        0  |  25   |   3    |   0   |   0
                           --------------------------------
                        1  |   1   |  118   |   1   |   0
                           ---------------------------------
                        2  |   4   |   22    |  85  |   2
                           ---------------------------------                       
                        3  |   0   |   0    |   1   |   45


### Building 3 Floor prediction

Performance and confusion matrix with model using Random Forest algorithm are shown below:

**Accuracy = 0.9514925**

**Kappa = 0.9339376**

**Confusion Matrix:**

                            |   0   |    1   |   2   |  3     |   4
                            ----------------------------------------
                         0  |  21   |   0    |   0   |   0    |   1
                            ----------------------------------------
                         1  |   3   |  110   |   0   |   0    |   0
                            ----------------------------------------
                         2  |   0   |   1    |  51   |   0    |   0
                            ----------------------------------------                       
                         3  |   0   |   0    |   3   |   40   |   5
                            ----------------------------------------                       
                         4  |   0   |   0    |   0   |   0    |   33
                         
                         
## Coordinates prediction (Regression with KNN model)

To predict the final position of a user connected to the internet from the signal strength, the regression models using KNN algorithm were used. Prediction of longitude and latitude require separate models, so finally there are 6 models (3 buildings x (longitude + latitude)).

### Building 1 Coordinates prediction

The performance of the model on validation set for Building 1 for longitude and latitude is shown below.

Longitude KNN model performance:
RMSE = 7.6099567, Rsquared = 0.9397686, MAE = 6.0526775

Latitude KNN model performance:
RMSE = 7.1845765, Rsquared = 0.9601064, MAE = 5.0841969

The visual result of predicted coordinates and the real ones can be seen in the following figure.

![building 1 result](https://user-images.githubusercontent.com/32273216/35474513-79480f44-038f-11e8-9d63-86317416a421.png)

The distribution of the distance error in meters is shown in the figure below.

![error distance building 1](https://user-images.githubusercontent.com/32273216/35739048-bd968374-0830-11e8-8b4e-dce516da2421.png)

### Building 2 Coordinates prediction

The performance of the model on validation set for Building 1 for longitude and latitude is shown below.

Longitude KNN model performance:
RMSE = 9.385072, Rsquared = 0.965634, MAE = 7.114257

Latitude KNN model performance:
RMSE = 11.4652267, Rsquared = 0.9069251, MAE = 8.1506292 

The visual result of predicted coordinates and the real ones can be seen in the following figure.

![building 2 result](https://user-images.githubusercontent.com/32273216/36063963-671bb2f8-0e84-11e8-9ef2-c3d136f9bcc0.png)

The distribution of the distance error in meters is shown in the figure below.

![building 2 errors](https://user-images.githubusercontent.com/32273216/36063980-73071c42-0e84-11e8-8491-f934d65ea62a.png)

### Building 3 Coordinates prediction

The performance of the model on validation set for Building 1 for longitude and latitude is shown below.

Longitude KNN model performance:
RMSE = 12.8063785, Rsquared = 0.8358643, MAE = 9.0960994 

Latitude KNN model performance:
RMSE = 10.7050057, Rsquared = 0.8955963, MAE = 8.1393196  

The visual result of predicted coordinates and the real ones can be seen in the following figure.

![building 3 results](https://user-images.githubusercontent.com/32273216/36063996-bd3e67c0-0e84-11e8-9506-b424022d93ce.png)

The distribution of the distance error in meters is shown in the figure below.

![building 3 errors](https://user-images.githubusercontent.com/32273216/36064001-d0632c00-0e84-11e8-8e2a-c8327cd4f646.png)
