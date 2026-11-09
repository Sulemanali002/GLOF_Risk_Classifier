GLOF Impact Classifier: Gilgit-Baltistan, Pakistan

A small machine learning project that classifies whether a recorded Glacial Lake Outburst Flood (GLOF) event in Gilgit-Baltistan had any recorded impact.

About this project

I am from Gilgit-Baltistan, a mountain region where GLOFs are a real danger for villages. I built this project to learn applied machine learning on a topic close to home. It is a learning project, not a hazard prediction tool.

Data
Source: High Mountain Asia GLOF Database (HMAGLOFDB), included here as HMAGLOFDB.csv
Used: 137 recorded GLOF events in Gilgit-Baltistan
Target: whether the event has any recorded impact (yes or no)
Method
Filtered the database to Gilgit-Baltistan
Built the yes/no impact label from the recorded impact columns
Encoded categorical features
Trained four models: Gradient Boosting, Random Forest, Decision Tree and Logistic Regression
Used leave-one-out cross-validation, because the dataset is small
Compared every model with a baseline that always predicts the most common class
Results
Model	Accuracy (leave-one-out CV)
Gradient Boosting	70.1%
Random Forest	69.3%
Decision Tree	69.3%
Logistic Regression	59.9%
Baseline (most common class)	55.5%
Important limitation

Most of the models' skill comes from location and year. This means the models mostly learn where and when events were documented, not the physical hazard of a lake. The results show that the models beat the baseline by about 15 points. They do not show that the models can predict real flood risk.

Charts

(Keep your existing chart images here: event locations, events per year, main causes, feature importance, model comparison. Add the baseline to the model comparison chart if you can.)

Tools

Python, pandas, scikit-learn, matplotlib, seaborn, Jupyter Notebook

Next steps
Add physical lake data from satellite imagery (lake area and change over time)
Test on a spatial split, so test events are in different places from training events
Try this on more regions of High Mountain Asia
