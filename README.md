# 🌾 Crop Yield Prediction using Machine Learning

Predicts **how much a crop will produce per hectare (yield)** in an Indian state from details like crop, season, state, year, area, rainfall, fertilizer and pesticide. Built in Google Colab with regression models, plus a small web app that fetches rainfall automatically from a weather API.

---

## 📌 Problem
Farmers, traders and government planners need a harvest estimate **before** harvest for storage, pricing and food-security planning. Yield depends on many factors at once, so it is hard to estimate by hand. A machine learning model can learn these patterns from 24 years of past records.

## 📊 Dataset
- **Source:** [Agricultural Crop Yield in Indian States Dataset](https://www.kaggle.com/datasets/akshatgupta7/crop-yield-in-indian-states-dataset) (Kaggle, by Akshat Gupta)
- **Size:** 19,689 rows × 10 columns, 1997–2020, 30 states/UTs, 55 crops
- **Columns:** Crop, Crop_Year, Season, State, Area, Production, Annual_Rainfall, Fertilizer, Pesticide, Yield
- **Target:** `Yield` (tonnes per hectare)

## ⚙️ Workflow
1. **Load and explore** the CSV with pandas (no missing values).
2. **Clean the data**
   - Removed `Production`: Yield = Production ÷ Area, so keeping it would leak the answer (data leakage).
   - Removed **Coconut**: it is measured in number of nuts, not tonnes.
   - Removed **impossible yields (≥ 170 t/ha)**: typing errors such as Maize 989 t/ha. Sugarcane up to ~165 t/ha is genuine, so it was kept.
3. **One-hot encode** Crop, Season and State (text → 1/0 columns).
4. **Split** into 80% training and 20% testing (`random_state=42`).
5. **Train** Linear Regression (baseline) and Random Forest.
6. **Evaluate** with R² on the test set and 5-fold cross-validation.
7. **Visualise** results and build a prediction function + web app.

## 📈 Results

| Model | R² (20% test set) |
|---|---|
| Linear Regression (baseline) | 0.74 |
| **Random Forest** | **0.95** |
| Random Forest, 5-fold cross-validation (average) | 0.XX |

**Biggest improvement:** data cleaning. Removing impossible yield values raised the Random Forest R² from about 0.60 to 0.95.

| Model comparison | Actual vs Predicted (Random Forest) |
|---|---|
| ![Model comparison](model_comparison.png) | ![Actual vs predicted](actual_vs_predicted.png) |

In the Actual vs Predicted plot, the red line is a perfect prediction. Most points sit close to it.

## 🌦️ Web app with live rainfall (add-on)
A **Gradio** web page lets anyone pick a crop, season, state and year and get a predicted yield. Rainfall is **not typed by the user**: the app fetches that year's daily rainfall for the state from the free **Open-Meteo Historical Weather API**, adds it up, and feeds it to the model.

Example: Rice, Kharif, Maharashtra, 1,000 ha → about **2.35 tonnes per hectare**.

## 🛠️ Tech stack
Python · Google Colab · pandas · scikit-learn · matplotlib · Gradio · requests · Open-Meteo API

## ▶️ How to run
1. Open `Crop_Yield_Prediction.ipynb` in Google Colab.
2. Run the first cell and upload `crop_yield.csv`.
3. Click **Runtime → Run all**. The last cell prints a public link to the web app.

## 📁 Files
```
crop-yield-predictor/
├── Crop_Yield_Prediction.ipynb   # full code
├── crop_yield.csv                # dataset
├── model_comparison.png          # bar chart of R² scores
├── actual_vs_predicted.png       # scatter plot
└── README.md
```

## ⚖️ Responsible AI and limitations
- **No personal data:** only state-level farming statistics.
- **Not fully representative:** some states and crops have few records; data is state-level, not per farm; Coconut is excluded.
- **Missing factors:** soil type, temperature, irrigation, pests and seed variety are not in the data.
- **API rainfall** is measured at one point per state, so it can differ from the official state average.
- Predictions are a **guide only**. Over-predicting yield could lead to poor planning, so they should not be the only basis for decisions.

## 🚀 Future scope
- Add temperature, soil and irrigation data.
- District-level predictions.
- Try stronger models such as XGBoost.
- Mobile app (APK) and a Marathi/Hindi interface for farmers.

## 🙏 Credits
- Dataset: [Akshat Gupta on Kaggle](https://www.kaggle.com/datasets/akshatgupta7/crop-yield-in-indian-states-dataset)
- Weather data by [Open-Meteo.com](https://open-meteo.com/)

## 👥 Team
Department of Electronics and Telecommunication Engineering, PVG's College of Engineering, Technology and Management, Pune

- Amey Chandge
- *(add teammates)*
