# Examining Popularity with DecisionTree

We will use unsupervised learning to group tracks by the features that carry the most influence over their popularity and then examine how the popularity changes based on those descriptors.

Notebooks:
[Model: Decision Tree Classifier](https://github.com/dmml-heriot-watt/group-coursework-machine-learners/blob/main/notebooks/models/Decision%20Tree/Decision_Tree_Model.ipynb)


#### Experimental design

The model of DecisionTree will be useful to classify the popularity of our dataset based on audio features of Spotify's songs as according to literature this model provide good performance. To carry out this model, it has been applied the next steps. 

1. **Data Preparation**: Firsly, it was neccesary to select the most relevant audio features to be considered in the training dataset. In this sense, using the technique of Recursive Feature Elimination staying like the most significant the followings: acousticness', 'liveness', 'loudness' and 'valence'. Moreover, the original dataset encompasses unbalanced data, therefore, it has to be applied specific techniques to achieve a balanced dataset to equilibrate the minority classes using the next methods: SMOTE(previously explained), SMOTETomek and RandomOverSample. These two lasts help to resample the unbalanced dataset.  

2. **Feature Scaling**: It is important to scale the information of the dataset to improve the performance of the model. To perform this, it was applied the standard, min-max and robust scaler methods. Adittionally, it was also considered the original data without feature scaling just for compairing the impact on it. As, it can see the original dataset is unbalanced and it may provoke low values in metrics of recall and F1

<p align="center">
  <img src="https://github.com/dmml-heriot-watt/group-coursework-machine-learners/raw/main/assets/Original_dataset.png" 
alt="Distribution plots" width="500" height="350">
</p>  

3. **Decision Tree Model**: This dataset (Spotify's songs) has unbalanced classes due to there is a majority class ('Low') y two others minitority class ('Medium' and 'High'). So, to address the condition of the dataset, it has been applied the model DecisionTreeClassifier considering different approaches in order to find the best evaluator. As first step, it necessary to balance the dataset applying resample techniques such as, SMOTE, SMOTETomek (SMK) and RandomOverSampler (ROS). As it can appreciate in the imabe below, with the resampled dataset it is better to apply algorithms for classifying.
   
<p align="center">
  <img src="https://github.com/dmml-heriot-watt/group-coursework-machine-learners/raw/main/assets/resampled_data.png" 
alt="Distribution plots" width="500" height="350">
</p>  
 Finally, we have considered a **ensemble method** called **Random Forest** which is based on the multiple decision tree.

4. **Final Aspects**: For every approach detailed in the model, it has been considered a variety of paramters in order to evaluate the model that provides the best performance for the dataset.   

#### Results

**1.** Feature scaling is important to improve the performance of the model; however, it has been demonstrated that there is no sensitive in DecisionTree model applying standardization method as their accuracy got in training dataset are almost similar with a gap of 2.1% (maximum gap) as much as test dataset. In the image below, for every resample technique (SMOTE, SMK and ROS) with different scaling methods there is hardly any gap in the accuracy among itself. Therefore, it allows to claim either unscaled the training dataset or with application, the results on the accuracy would be almost similar. 

<p align="center">
  <img src="https://github.com/dmml-heriot-watt/group-coursework-machine-learners/raw/main/assets/Feature_Scaling_Decision_Tree.png" alt="Distribution plots" width="800" height="350">
</p>

In this scenery, the RandomOverSample (ROS) technique provides the best accuracy; however, it tends to overfit the result of the dataset lacking of flexibility. Therefore, a model which provides better generalisation would be SMK; in other words, the selected resample technique is SMOTETomek with the method of Robust Scaler to improve the performance of the model. 

**2.** As second approach, it has been applied the DecisionTreeClassifier considering the dataset already resampled with the technique SMK and modifying the parameter "class_weight" of the model. It was taken a range from 1 to 80 to assess the best depth of the model also, achieving identify in which depth is presented the lowest error in test, then the class_weight is considered according to level of disproportion in the dataset between the majority class and its minority classes. 
Weight for Class ('Medium') : 29.77
Weight for Class ('High') : 1101

<p align="center">
  <img src="https://github.com/dmml-heriot-watt/group-coursework-machine-learners/raw/main/assets/DT_class_weight.png" 
alt="Distribution plots" width="500" height="350">
</p>

According to the image above, it appreciates that test error converges since max_depth = 50. To sum up this point, the table below shows the performance of this model with an accuracy of 90%. Additionally, other metrics such as precision and recall show high values for every class. 
|   | Precision | Recall | F1-Score | Support  |
|---|-----------|--------|----------|----------|
| Low   | 0.86 | 0.84 | 0.85 | 86,165 |
| Medium| 0.85 | 0.86 | 0.86 | 86,772 |
| High  | 0.98 | 0.99 | 0.99 | 87,103 |
|---|-----------|--------|----------|----------|
| Accuracy |     |       | 0.90     | 260,040 |
| Macro Avg| 0.90| 0.90  | 0.90     | 260,040 |
| Weighted Avg| 0.90 | 0.90 | 0.90  | 260,040 |

<details>
  <summary> *Confussion Matrix for Decision Tree modifying class_weights:</summary>
<p align="center">
  <img src="https://github.com/dmml-heriot-watt/group-coursework-machine-learners/raw/main/assets/Confusion Matrix _ DT_weight_class.png" 
alt="Distribution plots" width="500" height="350">
</p>  

</details>

**3.** The last approach is based on the application of ensemble method called random forest. This method comprises different decision tree and at the same time with feature randomness (subspace sampling). Thus, we get a robust model to make realible predictions. For this application it has taken into account the result of resampled technique (SMK) on the dataset. Definitely, this approach aims to the highest accuracy with an adequate sensibility in the prediction achieving a powerful model. 



#### Discussion
1) It is clear that combination between the RandomOverSample technique and the feature scaling of Robust Scaler providing the better accuracy (very near 1) is not the better option due to it is overfitting the design of the model. So, taking into account this aspect, the best selected model will be considering SMOTETomek technique and robust scaler as feature scaling as they, in conjunction, offer more flexibility to evaluate whatever input.
  #### Classification Report (SMOTETomek)
|   | Precision | Recall | F1-Score | Support  |
|---|-----------|--------|----------|----------|
| Low   | 0.86 | 0.82 | 0.84 | 86,146 |
| Medium| 0.84 | 0.87 | 0.86 | 86,332 |
| High  | 0.98 | 0.99 | 0.99 | 87,566 |
|---|-----------|--------|----------|----------|
| Accuracy |     |       | 0.90     | 392,650 |
| Macro Avg| 0.89| 0.89  | 0.89     | 392,650 |
| Weighted Avg| 0.90 | 0.90 | 0.90  | 392,650 |

<details>
  <summary> *Classification report for SMOTE and ROS:</summary>
  
As part of the research, it has the classification report using the other resample techniques.
  
     #### Classification Report (SMOTE)
|   | Precision | Recall | F1-Score | Support  |
|---|-----------|--------|----------|----------|
| Low   | 0.85 | 0.82 | 0.84 | 87,002 |
| Medium| 0.84 | 0.86 | 0.85 | 87,510 |
| High  | 0.98 | 0.99 | 0.99 | 87,255 |
|---|-----------|--------|----------|----------|
| Accuracy |     |       | 0.89     | 261,767 |
| Macro Avg| 0.89| 0.89  | 0.89     | 261,767 |
| Weighted Avg| 0.90 | 0.90 | 0.89  | 261,767 |

     #### Classification Report (ROS)
|   | Precision | Recall | F1-Score | Support  |
|---|-----------|--------|----------|----------|
| Low   | 0.99 | 0.92 | 0.95 | 87,002 |
| Medium| 0.93 | 0.99 | 0.96 | 87,510 |
| High  | 1.08 | 1.00 | 1.00 | 87,255 |
|---|-----------|--------|----------|----------|
| Accuracy |     |       | 0.97     | 261,767 |
| Macro Avg| 0.97| 0.97  | 0.97     | 261,767 |
| Weighted Avg| 0.97 | 0.97 | 0.97  | 261,767 |

</details>

  The accuracy is 90%, therefore we can claim the model has a good performance including similar precision values for every class. That is why the training dataset was resampled to avoid low precision for minority class like "MEDIUM" or "HIGH". In addition, the F1-score value with since 84% indicates that there is a good balance among the data and the obtained marks as Precision as Recall.

2) It is reliable to use the model of Decision Tree as classifier on this dataset as it allows to get best performance and also, the selected features such as acousticness', 'liveness', 'loudness' and 'valence' have contributed greatly to the performance being sufficient to get proper marks. So, for feature assessments, since the relevant audio features which has been mentioned, it will be neccesary just to predict the popularity of them.

3) According to the results, the random forest method is the best model basing on several decision tree as it provides the best accuracy and sensibility without overfitting the results. Although, it is well-known the random forest method is sufficiently robust to work with unbalanced data, the coefficient of the class give an idea of how much severe is this proportion regarding to majority class. That contribution of having a good proportion comes from the previous consideration of working with resampled dataset (technique: SMK).

#### Conclusion for Decision Tree
It is clear that DecisionTreeClassifier is a reliable model for applying in this dataset of spotify's songs as the accuracy got for every method overcomes 89%. This metric has been valid and considered reliable due to values about precision, recall and F1 are very representative (more than 85%). So, about this results we can claim RandomForest (ensemble method based on DecisionTree) presents the highest accuracy.
|   | Approach | Method | Result |
|---|-----------|--------|----------|
| Approach | 3 | RandomForest | 0.95 |
| Approach| 2 | DT_Class_Weights | 0.89 |
| Approach  | 1 | DT_RS_SMK	0 | 0.89 |
