# Starter prompts for creating model data

This file contains example prompts a beginner can paste as comments into notebook cells or send to an assistant (Copilot / ChatGPT) to get code and guidance for the `1-create-model-data.md` workflow.

Use these prompts in the notebook to generate code suggestions, or send them directly to an assistant. Start with prompts 1–4 to explore and clean the data, then continue to prompts 5–9 for feature engineering, modeling, and export.

---

## 1) Load & inspect the CSV
- Prompt:

  "Load the CSV at data/flights.csv into a pandas DataFrame (use low_memory=False). Show the first 10 rows, print the DataFrame shape, and print df.info()."

  What to expect: Code to import pandas and read the file, then display `df.head(10)`, `df.shape` and `df.info()`.

## 2) Summarize columns and types
- Prompt:

  "Give me a summary of each column: data type, number of unique values, sample values, and percent missing."

  What to expect: Code using `df.dtypes`, `df.nunique()`, `df.sample(5)` and `df.isnull().mean()`.

## 3) Quickly find and show missing values
- Prompt:

  "Show the number of nulls in each column sorted descending and display the rows where any nulls appear (limit to 20 rows)."

  What to expect: `df.isnull().sum().sort_values(ascending=False)` and `df[df.isnull().any(axis=1)].head(20)`.

## 4) Simple cleaning (replace nulls with zero)
- Prompt:

  "Fill numeric missing values with 0 and fill object/string missing values with an empty string. Then verify there are no remaining nulls."

  What to expect: Code using `df.select_dtypes`, `df.fillna(...)`, and `df.isnull().sum()` to verify.

## 5) Build an airport lookup CSV
- Prompt:

  "Create a CSV containing unique airports from `origin` and `dest` columns, assign each airport a unique integer id, and save it as `data/airports_ids.csv` with columns ['airport', 'id']."

  What to expect: Code that concatenates unique origins and destinations, deduplicates, assigns integer ids, and writes the CSV.

## 6) Create features for modeling
- Prompt:

  "Create features needed for modeling: extract day-of-week from flight date (or use the dataset's day column), join airport ids for origin and dest, and one-hot or label-encode categorical features. Create target `is_delayed_15` where 1 means arrival delay > 15 minutes or the dataset's delay flag. Show a sample of X (features) and y (target)."

  What to expect: Code to parse dates, create `day_of_week`, merge airport ids, encode categoricals, and show sample `X` and `y`.

## 7) Baseline probability model by group
- Prompt:

  "Create a baseline model that computes the percent of flights delayed (>15 min) grouped by (`day_of_week`, `origin`) and show how to use that lookup to predict probability on new rows."

  What to expect: `groupby` aggregation producing a lookup table and example application to `df`.

## 8) Train a scikit-learn model and save it
- Prompt:

  "Train a LogisticRegression or RandomForestClassifier to predict the delay flag. Split into train/test, report accuracy and ROC AUC, then save the trained model to `models/delay_model.joblib`."

  What to expect: `train_test_split`, `model.fit`, `metrics` and `joblib.dump` to save the model.

## 9) Export cleaned data and artifacts
- Prompt:

  "Save the cleaned DataFrame as `data/flights_cleaned.csv`, save the airport lookup as `data/airports_ids.csv`, and save the trained model to `models/delay_model.joblib`. Create directories if needed."

  What to expect: `df.to_csv`, directory creation via `os.makedirs(..., exist_ok=True)`, and `joblib.dump`.

## 10) Troubleshooting prompts

- Prompt:

  "I get an error reading the CSV: [paste error message]. Explain what's wrong and suggest code fixes (dtype overrides, encoding, skiprows, low_memory options)."

  Note: Paste the exact traceback for the best help.

## 11) Validation and edge-case checklist
- Prompt:

  "List likely edge cases (missing timestamps, airports with few flights, extreme outliers, class imbalance) and suggest mitigation strategies for each."

  What to expect: A numbered checklist with mitigation ideas (SMOTE or reweighting, grouping rare airports, clipping outliers, minimum-sample filters).

---

If you'd like, I can add a starter code cell to the notebook `manage-flight-data.ipynb` that runs prompt 1 and displays the initial summaries. Ask for "insert load cell" and I'll add it in notebook JSON format.
