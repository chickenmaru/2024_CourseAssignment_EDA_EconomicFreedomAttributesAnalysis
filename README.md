# Overview

# Flow
## 1.Simple Explanation of Dataset and All Attributes' Meanings


## 2.Keep/set ISO_code and countries as meta attributes, then use Feature Statistics to analyze and explain the value ranges and missing value proportions of the other columns. (10%)

By sorting the missing values in descending order, we can identify the attributes with more missing values as the following attributes. The attribute with the most missing values is 2h_reliability_p, with up to 56% of the columns being missing values.

<img width="865" height="465" alt="image" src="https://github.com/user-attachments/assets/2f52e695-940f-45b3-855e-089fa0aaf203" />

## 3.Delete Data from 2004 (Inclusive) and Earlier, and Compare the Difference in Missing Value Counts Between Data Before and After 2004. (10%)
Use the Select Rows tool to perform the deletion operation (as shown in the figure below).

<img width="865" height="616" alt="image" src="https://github.com/user-attachments/assets/0973a14e-2235-4c3c-a7d7-aca53dd878df" />

Using the same method, delete data after 2005 to obtain data before 2004, thereby comparing the difference in missing value counts between data before and after 2004. The left figure below shows the related information for data after 2004, where missing values account for 8.6%; the right figure shows the related information for data before 2004, where missing values account for 41.6%. The two have a very significant difference in missing value counts.

<img width="879" height="203" alt="image" src="https://github.com/user-attachments/assets/be7ae096-152c-430b-b13d-d6b8529b5c05" />

## 4.

