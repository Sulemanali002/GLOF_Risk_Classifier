# GLOF Risk Analysis: Gilgit-Baltistan, Pakistan

Can basic information about a glacier lake outburst flood (GLOF) event, namely where it happened, when, lake type and trigger, predict whether the event had **any recorded impact**? This project explores that question for Gilgit-Baltistan using the HMAGLOFDB database.

The honest answer: only modestly. The best models reach about **70%** accuracy against a **55.5%** baseline, and most of that skill comes from location and year, which likely reflects where and when events were documented, not a physical hazard model.

## Data

HMAGLOFDB, a database of historical GLOF events in High Mountain Asia: 703 events in the file used here, 137 of them in Gilgit-Baltistan.

**Target:** `AnyImpact` = 1 if the database records any impact for the event (lives, injuries, displacement, livestock, buildings, infrastructure, farmland, economic damage), otherwise 0. 61 of 137 events (45%) have a recorded impact.

**Important:** 0 means "no impact recorded", not "no impact happened".

**Data credit:** Shrestha, F., Steiner, J. F., Shrestha, R., Dhungel, Y., Joshi, S. P., Inglis, S., Ashraf, A., Wali, S., Walizada, K. M., Zhang, T. (2023). A comprehensive and version-controlled database of glacial lake outburst floods in High Mountain Asia. *Earth System Science Data*, 15, 3941-3961. https://doi.org/10.5194/essd-15-3941-2023. Database: https://doi.org/10.5281/zenodo.7271187

## Method

- Features: lake type, GLOF trigger (driver), year, latitude, longitude. Month was dropped (about 64% missing).
- Four models: Random Forest, Gradient Boosting, Decision Tree, Logistic Regression. 
- Evaluation: Leave-One-Out cross-validation (the scaler is refit inside every fold), compared with a majority-class baseline.
- Robustness checks: 200 random train/test splits and feature-removal tests.

## What the data looks like

### How much information is missing?
Many fields are mostly empty, which limits what any model can learn. Month is about 64% missing and the trigger is unknown for 123 of 137 events.

![Missing data](figures/missing_data.png)

### When did events happen?
![Events per decade](figures/events_per_decade.png)

The 2020 bar is low because that decade is not finished.

### Which months?
Only 49 of 137 events have a known month. Of those, 32 happened in May to July.

![Events by month](figures/events_by_month.png)

### Lake types and triggers
Ice-dammed lakes are the largest group (76 events). The trigger is unknown for most events, so conclusions about causes are not possible.

![Lake type and driver](figures/lake_type_and_driver.png)

### Which impacts were recorded?
Damage to infrastructure, buildings and farmland is recorded far more often than deaths.

![Impact types](figures/impact_types.png)

### Where are the events?
The dashed box is a rough study area around Ghanche (not the exact district border). It contains 3 recorded outbursts (1990, 1993, 2000) from the same lake in the Hushe basin, so they appear as a single point.

![Event map](figures/event_map.png)

### Did recorded impact change over time?
The share of events with a recorded impact differs strongly between periods (about 35% before 1950, 64% in 1950-79, about 44% after 1980). This points to differences in documentation, not only in danger.

![Impact by period](figures/impact_by_period.png)

## Results

| Model | LOOCV accuracy |
|---|---|
| Gradient Boosting | 70.1% |
| Random Forest | 69.3% |
| Decision Tree | 69.3% |
| Logistic Regression | 59.9% |
| Majority-class baseline | 55.5% |

![Model comparison](figures/model_comparison.png)

Tree-based models beat the baseline by about 14 points. Logistic Regression barely beats it. Differences of one or two points between models are within noise at this sample size.

### Where the best model goes wrong
Gradient Boosting correctly flags 36 of 61 events with recorded impact (recall 0.59) and 60 of 76 events without (recall 0.79).

![Confusion matrix](figures/confusion_matrix.png)

### One train/test split can mislead
Over 200 random 80/20 splits, Gradient Boosting scored between 50.0% and 89.3% (mean 69.5%). Only 4.0% of splits reached 85.7%. Cross-validated results are reported instead of a single split.

![Split instability](figures/split_instability.png)

### What is the model using?
Using location alone gives about 65 to 67%, so most of the skill comes from where events were recorded. The trigger adds nothing.

![Feature ablation](figures/feature_ablation.png)

## Limitations

- Small dataset (137 events). Results can change a lot with a few events.
- "No impact recorded" does not always mean no impact happened. Records are biased toward places and periods that were documented.
- Most of the skill comes from location and year, so the models mostly learn where and when events were documented. This is not a physical model of GLOF hazard.
- The trigger is unknown for most events and cannot be studied properly.
- Lake type and trigger are encoded with simple label encoding, which suits tree models more than Logistic Regression.
- The Ghanche study box contains only one lake with 3 recorded outbursts. Nothing here predicts risk for a specific village.

## Possible next steps

- Add physical data (glacier area, lake area, slope, temperature) from other sources.
- Use a larger regional dataset and validate across regions.
- Predict impact severity once more records are available.

## How to run

1. Put `glof.ipynb` and `HMAGLOFDB.csv` in the same folder. If the CSV is not included in this repository, download it from the Zenodo link above.
2. Open the notebook in Jupyter and choose Restart and Run All.
3. Charts are saved in the `figures/` folder.

Requires Python 3 with pandas, numpy, matplotlib and scikit-learn.

## Author

Suleman Ali, BS Computer Science, University of Baltistan, Skardu.
