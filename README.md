# Overview

# Flow
## 1.Simple Explanation of Dataset and All Attributes' Meanings


## 2.Keep/set ISO_code and countries as meta attributes, then use Feature Statistics to analyze and explain the value ranges and missing value proportions of the other columns. 

By sorting the missing values in descending order, we can identify the attributes with more missing values as the following attributes. The attribute with the most missing values is 2h_reliability_p, with up to 56% of the columns being missing values.

<img width="865" height="465" alt="image" src="https://github.com/user-attachments/assets/2f52e695-940f-45b3-855e-089fa0aaf203" />

## 3.Delete Data from 2004 (Inclusive) and Earlier, and Compare the Difference in Missing Value Counts Between Data Before and After 2004. 
Use the Select Rows tool to perform the deletion operation (as shown in the figure below).

<img width="865" height="616" alt="image" src="https://github.com/user-attachments/assets/0973a14e-2235-4c3c-a7d7-aca53dd878df" />

Using the same method, delete data after 2005 to obtain data before 2004, thereby comparing the difference in missing value counts between data before and after 2004. The left figure below shows the related information for data after 2004, where missing values account for 8.6%; the right figure shows the related information for data before 2004, where missing values account for 41.6%. The two have a very significant difference in missing value counts.

<img width="879" height="203" alt="image" src="https://github.com/user-attachments/assets/be7ae096-152c-430b-b13d-d6b8529b5c05" />

## 4.Delete all instances where Economic Freedom is missing, then use the column mean to fill in the missing values for each column.
Use the "is defined" in Select Column to find defined values.

<img width="820" height="589" alt="image" src="https://github.com/user-attachments/assets/cd3991ca-5729-46ed-8dd9-e7aec36666ca" />

Next, use the Impute tool to fill in the missing values in the column, where the filled value is the mean of the attribute, as shown in the figure below.

<img width="805" height="761" alt="image" src="https://github.com/user-attachments/assets/153237c7-ad40-415d-adcc-a255ae8b099b" />

Finally, add a Data Table to confirm if there are still any missing values present.

<img width="768" height="339" alt="image" src="https://github.com/user-attachments/assets/2a0dac68-7dd9-4e84-8acb-c25319c9ca36" />

## 5.Set year and rank as meta attributes; set 3d_freedom_own_foreign_currency as target.

Use the Select Column tool to adjust the specified attributes to target and meta attributes, as shown in the figure below.

<img width="865" height="906" alt="image" src="https://github.com/user-attachments/assets/32e711c8-d32b-4517-9b84-797c217d0bca" />

## 6.Split the data into 70% training and 30% testing, establish a Decision Tree model. Then, for different combinations of the two parameters "Min number of instances in leaves" and "Do not split subsets smaller than", respectively explore whether overfitting occurs.

The first step requires changing the target from numeric to categorical via the "edit domain" function; only then will the resulting prediction performance be usable within the decision tree.

<img width="865" height="746" alt="image" src="https://github.com/user-attachments/assets/b1d43b41-8fe0-4103-ba64-a1c8a967c237" />

The completed flowchart is shown in the figure below.

<img width="865" height="530" alt="image" src="https://github.com/user-attachments/assets/ab223570-c516-420c-b12e-500541a4d543" />

For decision trees, if the performance on the training set is good but the performance on the test set is poor, it is very likely due to the phenomenon of overfitting during training. We can observe the distribution of predicted versus actual combinations through the confusion matrix. If the accuracy between the test set and training set differs too much, or if the training set accuracy is very high while the test set accuracy is low, it indicates that overfitting has occurred. Therefore, we can use the CA value in the prediction to observe the accuracy of the confusion matrix, which can serve as an indicator for our judgment. If overfitting truly occurs, we can also examine the main issues from the confusion matrix chart (for example, assuming the data is divided into four classes, if the decision tree has a particularly high error rate when predicting the third class, we can start addressing it from there). Additionally, if the number of decision tree nodes is too large, there is also a certain probability that it is caused by overfitting, because the more precise the model's classification on the training set, the more nodes it will require. Therefore, in subsequent steps, we will refer to these three indicators—confusion matrix, CA, and number of nodes—as criteria for determining whether the trained model has overfitting.

After incorporating the data preprocessing conditions for questions 3-5, the flowchart of the established decision tree model is as follows. Under default conditions, with Min number of instances in leaves set to 2 and Do not split subsets smaller than set to 5, the generated decision tree has a training set CA value of 0.984, a test set CA value of 0.911, and 105 nodes in the tree.

<img width="774" height="1086" alt="image" src="https://github.com/user-attachments/assets/6e68a33b-f246-4b78-80a6-5ad5a5998fa7" />

The following lists the data from tests with other values, and a conclusion analysis will be conducted at the end.

### Test 1
Min number of instances in leaves set to 3
Do not split subsets smaller than set to 5
Training set CA value is 0.981
Test set CA value is 0.935
The generated decision tree has 103 nodes

<img width="734" height="1051" alt="image" src="https://github.com/user-attachments/assets/5cbdb9f9-4fe7-4b3e-a8ef-4619c0142027" />

### Test 2
Min number of instances in leaves set to 4
Do not split subsets smaller than set to 5
Training set CA value is 0.975
Test set CA value is 0.906
The generated decision tree has 97 nodes

<img width="733" height="1045" alt="image" src="https://github.com/user-attachments/assets/9ea0534e-ed49-45cb-b872-b569f7113773" />

### Test 3
Min number of instances in leaves set to 2
Do not split subsets smaller than set to 4
Training set CA value is 0.984
Test set CA value is 0.914
The generated decision tree has 111 nodes

<img width="769" height="1087" alt="image" src="https://github.com/user-attachments/assets/06ad55e5-f884-4899-8734-c48d65302e92" />

Conclusion: After multiple tests, it was found that the test set accuracy is approximately 5-8% lower than the training set CA value, but the CA value remains close to 1, indicating that the accuracy still reaches over 90%. Therefore, we infer that from the perspective of the CA value, there is relatively no overfitting situation.
Upon observing the confusion matrix, it can be found that the model's predictions for the value 5.0 are relatively weak. Therefore, if aiming to increase accuracy, we can consider addressing this characteristic.
Although the established decision tree has over 90-100 nodes, including 40-50 leaf nodes, after allocation, it can still be simply calculated that each leaf node accommodates approximately 30 data points (if overfitting occurs, the data allocated to each leaf node should be close to the set value of "Do not split subsets smaller than"; however, in the above tests, the average allocated number is about 5-6 times the minimum value, leading to the inference of no overfitting).

## 7.Build Decision Trees using training data ratios of 60%, 65%, 70%, 75%, 80%, 85%, and 90% respectively, store the data splitting ratios and CA values in a data table, and then plot a line chart.

The flowchart after data splitting is shown in the figure below.

<img width="688" height="573" alt="image" src="https://github.com/user-attachments/assets/831b7fd3-2812-4073-a605-d90992877446" />

The generated data is as follows.

<img width="636" height="645" alt="image" src="https://github.com/user-attachments/assets/d5b84aa2-36f5-4799-a60e-54fbe339d6d4" />


The generated line chart is as follows.
<img width="693" height="555" alt="image" src="https://github.com/user-attachments/assets/d413ad1d-b19d-4e0d-bb47-b6d39d7a6541" />


















