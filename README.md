# Medical Insurance Premium Prediction

## Project Overview
This project builds a regression model that predicts the charges billed by a health insurer for an individual, using seven recorded attributes: age, sex, BMI, number of children, smoker status, and region. The workflow covers data inspection, encoding of categorical variables, correlation analysis, model training, hyperparameter tuning with cross-validation, and evaluation on a held-out test set

The tuned Random Forest explains about **84% of the variance** in charges on the test set (R-squared approximately 0.84) with a mean absolute error of roughly **$2,600**, improving on the default model's R-sqaured of 0.80. Smoking status is by far the strongest single relationship with charges: smokers' average charges are about** 3.8 times **those of non-smokers.

## Dataset
- **Size:** 1,338 rows × 7 columns, with no missing values.
- **Target variable:** charges (individual medical costs billed by the insurer, median is $9,382 and the standard deviation is $12,110).

| Column | Type | Description |
|---|---|---|
| age | integer | Age of the peerson (18-64) |
| sex | category | male or female |
| bmi | float | Body mass index |
| children | integer | Number of dependents (0-5) |
| smoker | category | yes or no |
| region | category | southeast, southwest, northwest, northeast |
| charges | float | Medical charges billed |

## Methodogy
1. Inspection: Checked column typed and category counts. Confirmed there were no missing values.
2. Encoding categorical variables:
   - 'sex' mapped a binary value (female = 1, male = 0)
   - 'smoker' mapped to a binary value (yes = 1, no = 0)
   - 'region' one-hot encoded into four 0/1 columns (southeast, southwest, northwest, northeast)
3. Exploratory analysis: histograms of every feature and a correlation heatmap.
4. Train/test split: 80% training, 20% testing with X as all columns except 'charges' and y as charges.
5. Modeling: Random Forest.
6. Hyperparameter tuning: GridSearchCV with 5-fold cross-validation over 'max_depth', 'min_samples_split', and 'min_samples_leaf', run twice.
7. Evaluation: R-squared, RMSE, and MAE on the held-out test set, a predicted vs actual scatter plot, and a feature-importance chart.

## Key Findings

**Correlation with charges**
| Feature | Correlation with 'charges' |
|---|---|
| smoker | 0.787 |
| age | 0.299 |
| bmi | 0.198 |
| children | 0.0.68 |
| sex | -0.057 |

- Smoker status shows a strong positive relationship with charges.
- **Smoker vs. non-smoker:** Average charges for smokers are about 3.8x the average for non-smokers.

### Model Results
All metrics are computed on the 20% held-out test set. For context, the standard deviation of charges is about $12,110, so an RMSE of about $4,700 is well below the natural spread of the data.

| Model | Tuning | R-squared | RMSE | MAE |
|---|---|---|---|---|
| Random Forest (default) | None | 0.8 | 5,310 | 3,006 |
| Random Forest (tuned, grid 1) | Scored by R-squared. max_depth=5, min_samples_leaf=6, min_samples_split=8 | 0.844 | 4,684 | 2,563 |
| Random Forest (tuned, grid 2) | Scored by MAE; max_depth=8, min_samples_leaf=6, min_samples_split=6 | 0.841 | 4,739 | 2,581 |

## Limitations
- The metrics come from one 80/20 split rather than repeated cross-validation of the final model.
- Both grid searches used cross-validation on the training set, but the two tuned models were then compared on the same test set, so the test scores are slightly optimistic.
- Only Random Forest was trained. Other models were not compared.
- Small and simple dataset.


