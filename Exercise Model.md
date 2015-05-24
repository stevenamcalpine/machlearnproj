# machlearnproj---
title: "Predicting Exercise Type"
author: "Steven M"
date: "Sunday, May 24, 2015"
output: pdf_document
---

Before creating a model to predict the type of exercise performed by the test subjects, I performed some exploratory analysis. First, what classes exist:

```{r}
pmltraining<-'https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv'
pmltesting<-'https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv'
```

```{r}
table(pmltraining$classe)
```

then, what variables have variation and do not have missing values. 

```{r}
View(pmltraining)
head(pmltraining)
nsv<-nearZeroVar(pmltraining,saveMetrics=TRUE)
nsv
```

I selected many of the the potential feature variables into one variable:
```{r}
predictors<-c (    'roll_belt',  'pitch_belt',	'yaw_belt',	'total_accel_belt',	'gyros_belt_x',	'gyros_belt_y',	'gyros_belt_z',	'accel_belt_x',	'accel_belt_y',	'accel_belt_z',	'magnet_belt_x',	'magnet_belt_y',	'magnet_belt_z',	'roll_arm',	'pitch_arm',	'yaw_arm',	'total_accel_arm',	'gyros_arm_x',	'gyros_arm_y',	'gyros_arm_z',	'accel_arm_x',	'accel_arm_y',	'accel_arm_z',	'magnet_arm_x',	'magnet_arm_y',	'magnet_arm_z',	'roll_dumbbell',	'pitch_dumbbell',	'yaw_dumbbell',	'gyros_dumbbell_x',	'gyros_dumbbell_y',	'gyros_dumbbell_z',	'accel_dumbbell_x',	'accel_dumbbell_y',	'accel_dumbbell_z',	'magnet_dumbbell_x',	'magnet_dumbbell_y',	'magnet_dumbbell_z',	'gyros_forearm_x',	'gyros_forearm_y',	'gyros_forearm_z',	'accel_forearm_x',	'accel_forearm_y',	'accel_forearm_z',	'magnet_forearm_x',	'magnet_forearm_y',	'magnet_forearm_z'	)
```
I performed a decision tree analysis on a subset of these variables:
```{r,echo=FALSE}
exe_model<- train(pmltraining$classe ~ roll_belt +     pitch_belt +   yaw_belt + 	total_accel_belt + 	gyros_belt_y + 	gyros_belt_z + 	accel_belt_z + 	magnet_arm_x + 	magnet_arm_y + 	 	accel_forearm_y + 	accel_forearm_z + 	magnet_forearm_x ,method='rpart', data=pmltraining)
```
checked the prediction
```{r,echo=FALSE}
confusionMatrix(exe_model)
```
then predicted on the test set
```{r,echo=FALSE}
predict(exe_model,newdata=pmltesting)
```
