# Earthquake Magnitude Modeling with Deep Learning

**Magnitude Classification, Magnitude Regression, and Multi-Output Location Estimation**

An experimental research notebook exploring earthquake catalog records with dense neural networks, LSTMs, and linear regression. The work includes magnitude-threshold classification, numerical magnitude estimation, and joint latitude/longitude regression.

## Research Paper

[Read the related research paper on IEEE Xplore](https://ieeexplore.ieee.org/document/11064801)

The publication link was supplied by the project author. Its full text and publication metadata were not accessible during repository preparation, so this repository does not claim verified reproduction of all published results. Only the paper link is included; no paper PDF is bundled.

## Dataset Link

[USGS Earthquake Catalog - Search and CSV Download](https://earthquake.usgs.gov/earthquakes/search/)

[USGS ComCat Data Access](https://earthquake.usgs.gov/data/catalog/access/)

The supplied notebook reads a local file named `query (6).csv`. Its 22-column schema resembles a USGS catalog CSV export, including `time`, `latitude`, `longitude`, `depth`, `mag`, and uncertainty/source fields. The original CSV, query URL, date interval, geographical bounds, and magnitude filters were not supplied. The links above provide an official source for compatible catalog data; they are not a verified link to the exact original 1,873-row subset.

To obtain a compatible CSV:

1. Open the USGS catalog search page.
2. Select the required dates, magnitude range, and region.
3. Choose **CSV** under output options and run the search.
4. Save the exported file and update the notebook's CSV path.
5. Record the search filters and download date for reproducibility.

A newly downloaded export may produce different rows and results. See `data/README.md` for expected columns.

## Implemented Experiments

| Experiment | Task |
|---|---|
| Multi-output dense network | Longitude, latitude, and binary magnitude classification |
| Multi-output LSTM | Longitude, latitude, and binary magnitude classification |
| Simple LSTM | Binary magnitude classification |
| Linear Regression | Numerical magnitude estimation |
| Dense regression networks | Numerical magnitude estimation |

Binary labels use `mag > 4.7`. The notebook also explores label encoding, missing-value imputation, MinMax scaling, SMOTE, SelectKBest, confusion matrices, and ROC curves.

Although the original notebook heading mentions tsunami prediction, no explicit tsunami target or evaluated tsunami model is present. Imported bagging estimators and an ensemble heading do not establish an implemented ensemble experiment.

## Saved Results

These results are recorded in the supplied notebook and were not independently reproduced. They are exploratory values subject to the evaluation limitations below.

| Experiment | Saved result |
|---|---|
| Multi-output dense network, magnitude head | Accuracy: 85.66% |
| Multi-output LSTM, magnitude head | Accuracy: 78.76% |
| Simple LSTM classifier | Final validation accuracy: 84.46% |
| Linear Regression | MSE: 0.2359; R²: 0.7517 |
| Dense regression network with intermediate sigmoid layer | MSE: 0.1744; R²: 0.8164 |

R² is a regression score, not classification accuracy. Latitude and longitude losses operate on normalized coordinates. Some printed multi-output metric labels are incorrectly indexed, so location metrics should be recomputed by named outputs before use.

## Repository Contents

- `notebooks/Earthquake_Magnitude_Modeling.ipynb`: original notebook with saved outputs
- `data/README.md`: data source links, expected schema, and reproducibility notes
- `requirements.txt`: suggested legacy dependency versions
- `.gitignore`: excludes raw CSVs, environments, caches, and trained model artifacts

## Setup

The saved outputs indicate an older Python 3.10 / TensorFlow 2.x environment. The suggested requirements aim to support that legacy stack, including SciKeras and scikit-plot; they are not a verified lockfile.

```bash
python -m venv .venv
```

Activate the environment on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

Install dependencies and launch Jupyter:

```bash
pip install -r requirements.txt
jupyter lab
```

Open the notebook, replace the Google Drive CSV path with your local path, and omit Google Colab imports and Drive-mount calls when running locally. In Colab, mount Drive and point the loading cell to your file. Current Colab runtimes may need a legacy environment because this notebook predates newer Keras/SciPy releases.

## Execution Notes and Evaluation Limitations

The notebook is preserved as supplied and requires review before a clean sequential run:

- **Missing initialization:** the first call to `imputer.fit_transform(X)` occurs before `imputer` is defined. Initialize `SimpleImputer(strategy='mean')` before that cell.
- **Pre-split transformations:** encoding, scaling, SMOTE, and supervised feature selection occur before the train/test split. Fit preprocessing and feature selection on training data only and apply oversampling only within training data or cross-validation folds.
- **Test-set imputation:** one cell fits an imputer separately on test inputs. Transform test inputs using the training-fitted imputer instead.
- **Shared test/validation data:** the test split is passed to model training as validation data. Use a separate validation split and retain an untouched final test set.
- **Repeated SMOTE:** a second resampling call overwrites the feature table after location targets were extracted. Confirm feature/target alignment and whether latitude/longitude should be inputs for each task.
- **Metric indexing:** multi-output results are assigned to incorrect printed labels. Prefer named evaluation results and metrics computed directly against each target.
- **Time representation:** label-encoded timestamps are not physical elapsed-time features. Parse datetimes explicitly for chronological modeling.
- **LSTM sequences:** each sample is reshaped to one timestep. This does not demonstrate learning across sequences of prior earthquakes.
- **Prospective prediction:** catalog metadata can describe an event after it has occurred. These experiments do not establish advance prediction of when or where a future earthquake will occur. Define a forecast horizon and use only information available before that horizon for any prospective study.
- **Stateful execution:** variables such as `X_train`, `input_layer`, and `model` are reused across experiments. Review cell order and shapes before running the whole notebook.

## Validation Status

The notebook was read and its JSON structure and saved outputs inspected. No dataset was attached, so model training and end-to-end execution were not rerun. This is an exploratory research artifact, not an operational warning system.
