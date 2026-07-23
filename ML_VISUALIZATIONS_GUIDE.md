# ML Training Visualizations Guide

## Overview
The `train_ml_model_with_viz.py` script trains the Random Forest lead scoring model and generates 7 comprehensive visualizations to help you understand model performance.

---

## Quick Start

### 1. Install Required Dependencies
```bash
pip install matplotlib seaborn
```

Or install all dependencies:
```bash
pip install -r requirements.txt
```

### 2. Run Training Script
```bash
python train_ml_model_with_viz.py
```

### 3. View Results
All visualizations are saved to the `visualizations/` folder as high-resolution PNG files (300 DPI).

---

## Generated Visualizations

### 1. **Confusion Matrix** (`confusion_matrix.png`)
**What it shows:** How well the model predicts conversions vs non-conversions

**How to read:**
- **Top-left (True Negatives):** Correctly predicted non-conversions
- **Top-right (False Positives):** Incorrectly predicted as conversions
- **Bottom-left (False Negatives):** Missed conversions
- **Bottom-right (True Positives):** Correctly predicted conversions

**Good performance:** High numbers on diagonal (top-left and bottom-right), low numbers off-diagonal

---

### 2. **ROC Curve** (`roc_curve.png`)
**What it shows:** Model's ability to distinguish between converted and non-converted leads

**How to read:**
- **AUC (Area Under Curve):** Overall model quality
  - 0.9-1.0 = Excellent
  - 0.8-0.9 = Good
  - 0.7-0.8 = Fair
  - 0.5-0.7 = Poor
  - 0.5 = Random guessing
- **Orange line:** Your model's performance
- **Blue dashed line:** Random classifier baseline

**Good performance:** Curve hugs top-left corner, high AUC score

---

### 3. **Precision-Recall Curve** (`precision_recall_curve.png`)
**What it shows:** Trade-off between precision (accuracy of positive predictions) and recall (catching all positives)

**How to read:**
- **High precision:** When model says "will convert," it's usually right
- **High recall:** Model catches most leads that will convert
- **Curve shape:** Higher and to the right = better

**Use case:** Helps choose optimal threshold for your business needs

---

### 4. **Feature Importance** (`feature_importance.png`)
**What it shows:** Which features most influence conversion predictions

**How to read:**
- **Longer bars:** More important features
- **Typical ranking:**
  1. `lead_score` - Usually most important
  2. `last_contact_days_ago` - Recency matters
  3. `annual_revenue` - Company size indicator
  4. `industry_encoded` - Industry patterns
  5. `region_encoded` - Geographic patterns
  6. `has_current_tool` - Current tool usage

**Action:** Focus data collection efforts on high-importance features

---

### 5. **Learning Curves** (`learning_curves.png`)
**What it shows:** How model performance changes with more training data

**How to read:**
- **Blue line:** Training score (performance on training data)
- **Red line:** Validation score (performance on unseen data)
- **Gap between lines:**
  - Large gap = Overfitting (model memorizes training data)
  - Small gap = Good generalization
- **Plateau:** More data won't help much

**Action:** 
- If lines converge at low score → Need better features or model
- If large gap → Need more data or regularization
- If both high and converged → Model is good!

---

### 6. **Metrics Comparison** (`metrics_comparison.png`)
**What it shows:** Four key performance metrics side-by-side

**Metrics explained:**
- **Accuracy:** Overall correctness (all predictions)
- **Precision:** When model says "convert," how often is it right?
- **Recall:** Of all actual conversions, how many did we catch?
- **F1-Score:** Balanced measure of precision and recall

**Good performance:** All bars above 0.7 (70%)

**Business interpretation:**
- High precision, low recall → Conservative (misses opportunities)
- Low precision, high recall → Aggressive (wastes effort on bad leads)
- Balanced → Optimal for most use cases

---

### 7. **Class Distribution** (`class_distribution.png`)
**What it shows:** Balance of converted vs non-converted leads in training data

**How to read:**
- **Red bar:** Non-converted leads
- **Green bar:** Converted leads
- **Percentages:** Class balance

**Ideal:** 30-70% to 70-30% split (not too imbalanced)

**If imbalanced (e.g., 90-10):**
- Model may be biased toward majority class
- Consider collecting more minority class examples
- Use class weighting or resampling techniques

---

## Training Data

The script generates **100 synthetic training samples** with realistic patterns:

### Features Used:
1. **lead_score** (20-100): CRM lead quality score
2. **last_contact_days_ago** (5-300): Days since last contact
3. **annual_revenue** ($200K-$50M): Company revenue
4. **industry**: SaaS, Manufacturing, Retail, Technology, Finance, E-Commerce
5. **region**: North America, Europe, Asia, Africa
6. **current_tool**: Excel, Salesforce, HubSpot, Freshsales, No CRM Tool, Custom

### Conversion Logic:
- Higher lead scores → Higher conversion probability
- Recent contact (30+ days) → Higher conversion probability
- Larger revenue → Higher conversion probability
- Tech/SaaS industries → Slight boost
- Random variation added for realism

---

## Understanding Model Performance

### Typical Results (with synthetic data):
- **Accuracy:** 70-85%
- **Precision:** 65-80%
- **Recall:** 60-75%
- **F1-Score:** 65-75%
- **AUC:** 0.75-0.85

### What's Good Enough?
- **For sales outreach:** 70%+ accuracy is solid
- **Better than random:** Anything above 50%
- **Better than rule-based:** Compare to old hardcoded logic

---

## Using Real Data

To train with your actual historical data:

### 1. Prepare CSV with these columns:
```csv
lead_score,last_contact_days_ago,annual_revenue,industry,region,current_tool,converted
85,45,5000000,SaaS,North America,Excel,1
60,120,1200000,Retail,Europe,Salesforce,0
...
```

### 2. Modify `train_ml_model_with_viz.py`:
Replace the `create_sample_training_data()` call with:
```python
training_data = pd.read_csv('data/historical_conversions.csv')
```

### 3. Run training:
```bash
python train_ml_model_with_viz.py
```

---

## Interpreting Results for Business Decisions

### Scenario 1: High Precision, Low Recall
**What it means:** Model is conservative - only flags high-confidence leads

**Business impact:**
- ✅ Emails sent are highly targeted
- ❌ Missing potential opportunities
- **Action:** Lower prediction threshold to catch more leads

### Scenario 2: Low Precision, High Recall
**What it means:** Model is aggressive - flags many leads

**Business impact:**
- ✅ Catching most conversion opportunities
- ❌ Wasting effort on low-quality leads
- **Action:** Raise prediction threshold to be more selective

### Scenario 3: Balanced Performance
**What it means:** Good trade-off between catching leads and avoiding waste

**Business impact:**
- ✅ Optimal for most sales teams
- **Action:** Monitor and adjust based on sales feedback

---

## Troubleshooting

### Issue: "ModuleNotFoundError: No module named 'matplotlib'"
**Solution:**
```bash
pip install matplotlib seaborn
```

### Issue: Low model performance (accuracy < 60%)
**Possible causes:**
1. Not enough training data (need 100+ samples)
2. Features don't predict conversions well
3. Data quality issues (missing values, errors)

**Solutions:**
- Collect more historical conversion data
- Add more predictive features
- Clean and validate data

### Issue: Large gap in learning curves
**Cause:** Overfitting - model memorizes training data

**Solutions:**
- Collect more training data
- Reduce model complexity
- Add regularization

### Issue: Imbalanced classes (90% one class)
**Cause:** Not enough examples of minority class

**Solutions:**
- Collect more minority class examples
- Use class weighting in model
- Try SMOTE or other resampling techniques

---

## Next Steps After Training

1. **Review visualizations** in `visualizations/` folder
2. **Check feature importance** - focus on top features
3. **Verify model file** exists at `models/lead_scorer.pkl`
4. **Start API server:**
   ```bash
   python api.py
   ```
5. **Test predictions** through frontend or API
6. **Monitor performance** with real leads
7. **Retrain periodically** with new conversion data

---

## File Locations

```
project/
├── train_ml_model_with_viz.py    # Training script
├── ml_lead_scorer.py              # Model implementation
├── models/
│   └── lead_scorer.pkl            # Trained model (created after training)
└── visualizations/                # Generated visualizations (created after training)
    ├── confusion_matrix.png
    ├── roc_curve.png
    ├── precision_recall_curve.png
    ├── feature_importance.png
    ├── learning_curves.png
    ├── metrics_comparison.png
    └── class_distribution.png
```

---

## Questions?

- **How often should I retrain?** Monthly or when you have 50+ new conversion examples
- **Can I use this with real data?** Yes! Replace synthetic data with your CSV
- **What if accuracy is low?** Normal with synthetic data. Real data performs better.
- **Do I need to retrain to use the API?** No, API works with existing model or falls back to rules

---

## Summary

✅ **7 visualizations** show complete model performance  
✅ **Easy to interpret** with business-focused explanations  
✅ **High-resolution** PNG files ready for reports/presentations  
✅ **Automated training** with one command  
✅ **Works with synthetic or real data**  

Run `python train_ml_model_with_viz.py` to get started!
