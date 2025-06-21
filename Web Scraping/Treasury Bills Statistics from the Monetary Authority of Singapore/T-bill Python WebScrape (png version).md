 ### Web Scraping Python code for Extracted MAS T-bill Statistics (png VERSION)
> This version produces a png file for presentation portability
> 
> View my [README](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/README.md) file for overall approach and code design 

### Screenshot image of the matpilot graph
>
> ![image](https://github.com/user-attachments/assets/af9e3765-e849-4069-a01d-7a089fa98b08)

### Below is the python code snippet:
```python
import requests
import csv
import json
import time
from datetime import datetime,timedelta
import matplotlib.pyplot as plt
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

def plot_yields(json_path):
    with open(json_path, "r", encoding="utf-8") as f:
        tbills = json.load(f)

    records = [
        (
            datetime.strptime(t["Issue Date"], "%Y-%m-%d"),
            float(t["Cut-off Yield (%)"])
        )
        for t in tbills # only includes records not blank or missing
        if t.get("Issue Date") and t.get("Cut-off Yield (%)") not in (None, "")
    ]# List comprehension

    if not records:
        print("[!] No yield data available for plotting.")
        return #graceful early exit if no yield data

    records.sort()
    dates, yields = zip(*records)

    plt.figure(figsize=(10, 5))
    plt.plot(dates, yields, marker="o", linestyle="-", color="teal")
    plt.title("Singapore T-bill Cut-off Yields Over Time")
    plt.xlabel("Issue Date")
    plt.ylabel("Cut-off Yield (%)")
    plt.xticks(rotation=45)
    plt.grid(True)
    plt.tight_layout()

    # Highlight peak and trough yields
    min_idx = yields.index(min(yields))
    max_idx = yields.index(max(yields))

    plt.annotate(f"Lowest: {yields[min_idx]:.2f}%", xy=(dates[min_idx], yields[min_idx]),
                 xytext=(dates[min_idx], yields[min_idx] - 0.4),
                 arrowprops=dict(arrowstyle="->", color="red"),
                 fontsize=9, color="red")

    plt.annotate(f"Highest: {yields[max_idx]:.2f}%", xy=(dates[max_idx], yields[max_idx]),
                 xytext=(dates[max_idx]+ timedelta(days=100), yields[max_idx] - 0.1),
                 arrowprops=dict(arrowstyle="->", color="green"),
                 fontsize=9, color="green")
    plt.savefig("t_bill_yields.png", dpi=300)
    plt.show()


if __name__ == "__main__":
    if USE_OFFLINE:
        print("[i] Using cached data from JSON file.")
        plot_yields("mas_singapore_tbills.json")
    else:
        data = fetch_all_tbills()
        if data:
            save_csv(data)
            save_json(data)
            plot_yields("mas_singapore_tbills.json")
        else:
            print("[!] No data found.")

    
