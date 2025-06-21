### Web Scraping Python code for Extracted MAS T-bill Statistics (HTML VERSION)
> This version produces a html file for an interactive yield chart allowing hovering.
> 
> View my [README](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/README.md) file for overall approach and code design 

### Click the video below for a quick preview of the html page
>
> [![Link to the video](https://github.com/user-attachments/assets/8522f3eb-7da4-4070-aa1b-ec4fa34a6d91)](https://youtu.be/qkEoILvvpcs)

### Below is the python code snippet:
```python



import requests
import csv
import json
import time
from datetime import datetime
import plotly.express as px
import pandas as pd

USE_OFFLINE = True  # Set to False to re-fetch from MAS API

API_URL = "https://eservices.mas.gov.sg/statistics/api/v1/bondsandbills/m/listauctionbondsandbills"
CSV_FILE = "mas_singapore_tbills.csv"
JSON_FILE = "mas_singapore_tbills.json"
HEADERS = {
    "User-Agent": "Mozilla/5.0",
    "Accept": "application/json"
}

def fetch_all_tbills():
    all_records = []
    start = 0
    rows_per_page = 25
    from_date = "2020-01-01"
    to_date = datetime.today().strftime("%Y-%m-%d")

    while True:
        params = {
            "rows": rows_per_page,
            "start": start,
            "filters": f'product_type:"B" AND auction_date:[{from_date} TO {to_date}]',
            "sort": "auction_date asc AND raw_tenor asc"
        }

        print(f"[→] Fetching records starting at {start}…")
        response = requests.get(API_URL, headers=HEADERS, params=params, timeout=20)
        response.raise_for_status() # This line will raise an error with faulty server interaction

        data = response.json() # parses response body as JSON and return in dict or list depending on JSON structure

        records = data.get("result", {}).get("records", [])
        if not records:
            print("[✓] No more records found.")
            break

        for rec in records:
            all_records.append({
                "Issue Code": rec.get("issue_code"),
                "ISIN Code": rec.get("isin_code"),
                "Issue Date": rec.get("issue_date"),
                "Maturity Date": rec.get("maturity_date"),
                "Cut-off Yield (%)": rec.get("cutoff_yield")
            })

        start += rows_per_page
        time.sleep(1.0)  # Polite pacing

    return all_records

def save_csv(data):
    with open(CSV_FILE, "w", newline="", encoding="utf-8") as f:
        writer = csv.DictWriter(f, fieldnames=data[0].keys()) #uses [0] line as header a.k.a your keys
        writer.writeheader()
        writer.writerows(data)
    print(f"[✓] Saved {len(data)} records to {CSV_FILE}")

def save_json(data):
    with open(JSON_FILE, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=2)
    print(f"[✓] Saved {len(data)} records to {JSON_FILE}")


def plot_yields_interactive(csv_file="mas_singapore_tbills.csv"):
    df = pd.read_csv(csv_file, parse_dates=["Issue Date"])
    df = df[df["Cut-off Yield (%)"].notnull()]  # filter out rows with null yields

    fig = px.line(
        df,
        x="Issue Date",
        y="Cut-off Yield (%)",
        markers=True,
        title="Singapore T-bill Cut-off Yields Over Time",
        hover_data={
            "Issue Code": True,
            "ISIN Code": False,
            "Maturity Date": False,
            "Cut-off Yield (%)": False  # We'll override this in the template
        }
    )

    fig.update_traces(
        hovertemplate="<br>".join([
            "Issue Date: %{x|%Y-%m-%d}",
            "Cut-off Yield: %{y:.2f}%"
        ])
    )

    fig.write_html("interactive_yield_plot.html")



if __name__ == "__main__":
    if USE_OFFLINE:
        print("[i] Using cached data from CSV.")
        plot_yields_interactive("mas_singapore_tbills.csv")

    else:
        data = fetch_all_tbills()
        if data:
            save_csv(data)
            save_json(data)
            plot_yields_interactive("mas_singapore_tbills.csv")

        else:
            print("[!] No data found.")
