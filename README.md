# E-Commerce Customer Segmentation (RFM + K-Means)

Segmenting 5,878 customers of a UK online retailer by **Recency, Frequency and Monetary value**, then turning the segments into marketing actions.

**Dataset:** [UCI Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii) (1.07M transaction lines, Dec 2009 - Dec 2011)
**Tools:** Python (pandas, scikit-learn, matplotlib, seaborn)

## Key findings

| Segment | Customers | Avg recency | Avg orders | Avg spend | Share of revenue |
|---|---|---|---|---|---|
| Champions | 1,188 (20%) | 27 days | 19.3 | £11,014 | **73.7%** |
| New / Promising | 1,251 (21%) | 28 days | 3.0 | £865 | 6.1% |
| At-Risk | 1,465 (25%) | 228 days | 5.1 | £2,002 | 16.5% |
| Lost | 1,974 (34%) | 396 days | 1.4 | £326 | 3.6% |

- 20% of customers generate about 74% of revenue.
- **Recommended target: At-Risk.** They spent £2.9M historically and have been silent for about 7 months, so a win-back campaign has real money behind it.

Full write-up: [`report.docx`](report.docx)

## Method

1. **Clean:** drop rows with no Customer ID (22.8%), cancelled invoices (prefix `C`) and non-positive quantity/price.
2. **Build RFM:** Recency (days since last purchase), Frequency (unique invoices), Monetary (total spend).
3. **Transform:** `log1p` + standardise, because all three metrics are heavily right-skewed.
4. **Cluster:** K-Means, k chosen with elbow and silhouette plots (k = 4).
5. **Name segments** from their average R/F/M so they make sense to a marketing manager.


## Project structure

```
online-retail-project/
├── dataset/        # online_retail_II cleaned.csv (could not upload original dataset file too large)
├── notes/          # analysis scripts
├── charts/         # generated charts
├── rfm_table.csv   # RFM score + segment for every customer
└── README.md
```

## Run it yourself

```bash
pip install pandas scikit-learn matplotlib seaborn
# download online_retail_II.csv from UCI/Kaggle into dataset/
python notes/uci.py
```

## Limitations

- 23% of lines had no Customer ID, so guest and wholesale behaviour is excluded.
- Recency is measured against the last date in the data (Dec 2011).
- Segments are behavioural only: no margin or product-category data.
