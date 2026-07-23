# Dataset Migration Guide

## Switching to sales_leads_dataset.csv

This guide explains how to use your new `sales_leads_dataset.csv` file with SalesAI.

---

## 🎯 Quick Start

### Option 1: Automatic (Recommended)
The system automatically detects and uses `sales_leads_dataset.csv` if it exists.

**Just run:**
```bash
python validate_dataset.py
```

Then start the application:
```bash
python api.py
```

### Option 2: Manual Verification
```bash
# 1. Validate the dataset
python validate_dataset.py

# 2. Test with CLI
python main.py

# 3. Start web interface
python api.py
```

---

## 📊 Dataset Format

### Required Columns

Your `sales_leads_dataset.csv` must have these columns:

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `name` | string | Customer name | "Atharv Bhardwaj" |
| `email` | string | Email address | "atharv@example.com" |
| `company` | string | Company name | "Acme Analytics" |
| `industry` | string | Industry sector | "SaaS" |
| `lead_score` | integer | Quality score (0-100) | 92 |
| `contact_days_ago` | integer | Days since last contact | 45 |
| `annual_revenue` | integer | Annual revenue in dollars | 1200000 |
| `current_tool` | string | Current CRM/tool | "Excel" |
| `region` | string | Geographic region | "North America" |

### Column Name Compatibility

The system supports both naming conventions:
- ✅ `contact_days_ago` (new dataset)
- ✅ `last_contact_days_ago` (old dataset)

Both work automatically!

---

## 🔄 What Changed

### Old Dataset (`customers.csv`)
```csv
name,email,company,industry,lead_score,last_contact_days_ago,annual_revenue,current_tool,region
Atharv Bhardwaj,atharvbhardwaj07@gmail.com,Acme Analytics,SaaS,92,45,1200000,Excel,North America
Bob Smith,bob@example.com,Smith & Co,Manufacturing,78,10,800000,None,Europe
```

**Issues:**
- Only 2 leads
- Limited for testing
- No variety

### New Dataset (`sales_leads_dataset.csv`)
```csv
name,email,company,industry,lead_score,contact_days_ago,annual_revenue,current_tool,region
Atharv Bhardwaj,atharvbhardwaj07@gmail.com,Acme Analytics,SaaS,92,45,1200000,Excel,North America
Sonal Bose,sonal.bose@databridge.com,DataBridge Inc,SaaS,21,112,16113608,No CRM Tool,Africa
Geeta Iyer,giyer@galaxy.in,Galaxy Digital,Retail,38,230,40044950,Freshsales,North India
... (more leads)
```

**Benefits:**
- ✅ More leads for testing
- ✅ Diverse industries
- ✅ Various regions
- ✅ Different lead scores
- ✅ Multiple CRM tools

---

## 🔍 Validation

### Run Validation Script
```bash
python validate_dataset.py
```

**Output:**
```
======================================================================
Sales Leads Dataset Validation
======================================================================

✓ Dataset found: data/sales_leads_dataset.csv

✓ Total leads: 50

Columns found:
  ✓ name
  ✓ email
  ✓ company
  ✓ industry
  ✓ lead_score
  ✓ contact_days_ago
  ✓ annual_revenue
  ✓ current_tool
  ✓ region

Dataset Statistics:
----------------------------------------------------------------------
  Lead Scores:
    • Average: 52.3
    • Min: 14
    • Max: 95
    • High quality (≥80): 8

  Industries (5 unique):
    • SaaS: 15
    • Finance: 12
    • Retail: 10
    • Education: 8
    • E-Commerce: 5

  Regions (4 unique):
    • North America: 20
    • Europe: 15
    • Asia: 10
    • Africa: 5

✅ Dataset validation complete!
```

---

## 🚀 Using the New Dataset

### 1. CLI Mode
```bash
python main.py
```

**Output:**
```
Loaded 50 customers from sales_leads_dataset.csv
SalesManager selected 8 customer(s) for outreach.

=== Working on customer: Atharv Bhardwaj (Acme Analytics) ===
Manager chose agent: value_focus
Reasoning: High lead score and SaaS industry...
```

### 2. Web Interface
```bash
python api.py
```

Then open `frontend/index.html` in your browser.

**You'll see:**
- All 50 leads in the sidebar
- Lead scores and selection badges
- ML conversion probabilities (if trained)
- Full customer details

### 3. API Endpoints
```bash
# Get all customers
curl http://localhost:8000/customers

# Get statistics
curl http://localhost:8000/stats
```

---

## 📈 ML Model Training

### Train with New Dataset

The ML model can learn from your new dataset:

```bash
python train_ml_model.py
```

**Note:** You'll need to add a `converted` column to your dataset for training:

```csv
name,email,...,converted
Atharv Bhardwaj,atharvbhardwaj07@gmail.com,...,1
Sonal Bose,sonal.bose@databridge.com,...,0
```

Where:
- `1` = Converted to customer
- `0` = Did not convert

---

## 🔧 Troubleshooting

### Issue 1: Dataset Not Found
```
❌ Dataset not found at: data/sales_leads_dataset.csv
```

**Solution:**
```bash
# Check file location
ls data/

# Should show:
# sales_leads_dataset.csv
```

### Issue 2: Invalid Column Names
```
Warning: Skipping invalid row: KeyError: 'contact_days_ago'
```

**Solution:**
- Check column names match exactly
- No extra spaces
- Correct spelling

### Issue 3: Invalid Data Types
```
Warning: Skipping invalid row: ValueError: invalid literal for int()
```

**Solution:**
- Ensure `lead_score` is a number (0-100)
- Ensure `contact_days_ago` is a number
- Ensure `annual_revenue` is a number
- No empty values in required fields

### Issue 4: Encoding Issues
```
UnicodeDecodeError: 'utf-8' codec can't decode byte...
```

**Solution:**
- Save CSV as UTF-8 encoding
- Remove special characters
- Use standard ASCII characters

---

## 📊 Dataset Statistics

### Lead Quality Distribution

After loading your dataset, check the statistics:

```bash
curl http://localhost:8000/stats
```

**Response:**
```json
{
  "total_customers": 50,
  "selected_for_outreach": 8,
  "selection_rate": 16.0,
  "average_lead_score": 52.3,
  "ml_scorer_available": true,
  "average_conversion_probability": 0.623,
  "high_probability_leads": 12
}
```

### Lead Selection Criteria

**Default Rules:**
- Lead score ≥ 80
- Last contact ≥ 14 days ago

**From your dataset:**
- High quality leads (≥80): 8 leads
- Ready for contact (≥14 days): ~40 leads
- **Selected for outreach:** 8 leads (meet both criteria)

---

## 🎯 Best Practices

### 1. Data Quality
- ✅ Valid email addresses
- ✅ Complete information
- ✅ Accurate lead scores
- ✅ Recent contact dates

### 2. Regular Updates
- Update `contact_days_ago` regularly
- Adjust lead scores based on engagement
- Add new leads as they come in
- Remove converted customers

### 3. Backup
```bash
# Backup your dataset
cp data/sales_leads_dataset.csv data/sales_leads_dataset_backup.csv
```

### 4. Version Control
```bash
# Track changes
git add data/sales_leads_dataset.csv
git commit -m "Updated lead dataset"
```

---

## 🔄 Migration Checklist

- [ ] Place `sales_leads_dataset.csv` in `data/` folder
- [ ] Run `python validate_dataset.py`
- [ ] Check validation output
- [ ] Fix any errors
- [ ] Test with `python main.py`
- [ ] Start API with `python api.py`
- [ ] Open frontend and verify leads appear
- [ ] Test email generation
- [ ] (Optional) Train ML model with historical data

---

## 📝 Example Dataset Row

```csv
name,email,company,industry,lead_score,contact_days_ago,annual_revenue,current_tool,region
Atharv Bhardwaj,atharvbhardwaj07@gmail.com,Acme Analytics,SaaS,92,45,1200000,Excel,North America
```

**Breakdown:**
- **Name:** Atharv Bhardwaj
- **Email:** atharvbhardwaj07@gmail.com (valid format)
- **Company:** Acme Analytics
- **Industry:** SaaS (high-value industry)
- **Lead Score:** 92 (high quality, ≥80)
- **Contact Days:** 45 (ready for contact, ≥14)
- **Revenue:** $1,200,000 (good size)
- **Current Tool:** Excel (has tool to replace)
- **Region:** North America

**Result:** ✅ Selected for outreach

---

## 🎉 Success!

Once validated, your new dataset is ready to use!

**What you can do:**
1. ✅ Generate emails for all leads
2. ✅ Use ML scoring (after training)
3. ✅ Filter by industry, region, score
4. ✅ Track conversion rates
5. ✅ Optimize lead selection

---

## 📚 Additional Resources

- **Validation Script:** `python validate_dataset.py`
- **ML Training:** `python train_ml_model.py`
- **API Documentation:** `ARCHITECTURE.md`
- **Error Handling:** `EMAIL_VALIDATION.md`

---

**Status:** ✅ Ready to Use

**Your dataset:** `data/sales_leads_dataset.csv`

**Next step:** Run `python validate_dataset.py` to get started!

---

*End of Dataset Migration Guide*
