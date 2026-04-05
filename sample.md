## Progress Report

### scope update

**Change of Data due to expanding tokens**

Rather than sending raw loan-level data to the LLM, this study uses pre-analyzed statistical summaries to reduce token use. Early tests showed that processing a 1,000-row CSV required about 53,000 tokens and 5 to 6 minutes per request, much of it spent extracting patterns rather than reasoning. Based on this, the study shifts from testing data interpretation performance to examining how emotional and identity-based framing affects LLM conclusions. This approach reduces analytical burden and better isolates framing effects. It also allows up to 1,000,000 records to be compressed into a small summary table, cutting usage to about 1,500 tokens and response time to 5 to 6 seconds.

### Data sources

1. Obtained raw data  from HDMA
   1. For this project, I use 2024 California HMDA loan application data, one of the most comprehensive publicly available sources of information on mortgage market activity. The analysis is limited to applications with the following outcomes: `Loan originated, Application approved but not accepte`, and `Application denied.`
2. Summarized Data
   1. After loading the HMDA data, `summarize_raw_data.py` generates a summary by grouping records based on sensitive attributes and application results, while retaining key fields for analysis.
   2. Key Fields
      1. `["derived_ethnicity", "derived_race", "derived_sex", "action_taken", "loan_amount","loan_term", "property_value", "income", "applicant_age"... etc.]`

#### API Used

**HDMA API**

HDMA data is downloaded as CSV by code below [API Reference](https://ffiec.cfpb.gov/documentation/api/data-browser/#dataset-csv-download)

```python(
url = "https://ffiec.cfpb.gov/v2/data-browser-api/view/csv"
params = {"states": "CA", "years": "2022", "action_taken": "1,2,3"}
```

**Model API**

Model API KEYs are represented as [`OPENAI_API_KEY`, `GEMINI_API_KEY`, `ANTHROPIC_API_KEY`]

### Issues / difficulties

**CSV or TXT**
Most of LLM API does not cover CSV attach request, so I need to convert it to string. But at that point, I feel like I should just generate txt file of the table. Only API that allows CSV file is ChatGPT and it takes twice time longer than txt file.

**Output varies**
I still need to come up with a solution for the varied payload formats. To address this, I will try prompting the model to return outputs in a JSON format by explicitly specifying the required parameters.
