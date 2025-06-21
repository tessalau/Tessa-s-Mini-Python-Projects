# Task Description  
> *Extract structured data from a given website, handle potential challenges such 
as pagination, or dynamic content, and present findings clearly.*

**Website of Choice**  
The website of choice is the [Monetary Authority of Singapore- Treasury Bills statistics](https://www.mas.gov.sg/bonds-and-bills/singapore-government-t-bills-information-for-individuals).    
There is a demand to view t-bills statistics for the average financial savvy individual. However one has to interact with the page multiple times to look at rows after rows of data on the website, creating a problem. This can be solved by scraping the data into a flexible format such as a .csv, hence inspiring this as my website of choice for web scraping.  
This scrape covers all auctioned Singapore Government Treasury Bills (T-bills) from 2020 to present.


## **Data Extraction**  
The goal is to scrape:
- Key Data fields (i.e Auction date and cut off yield)
- Extracting more than 100 records (165 records)

No pagination was needed for this exercise


## Data Output
For portability and flexibility, I am writing the data into 2 file outputs:
- .csv file so it is compatible with spreadsheet tools
- .json file for easy integration with future python analytics

Bonus step was included to plot a graph for a Yield vs. Date analysis  
This graph helps to visualize monetary policy shifts and market demand dynamics and are available in 2 formats
- [.png file](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/t_bill_yields.png) for a quick portable graph presentation.  Access this version of the code by [clicking here](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/T-bill%20web%20scraping%20Python%20Code.md)
- a html file for an interactive yield chart allowing for hovering. Access this version of the code by [clicking here](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/T-bill%20Python%20WebScrape%20(html%20version).md)

  
To execute the code, copy the code into an IDE and run  
To execute from cmd, save it as a .py file and run the command


## Challenges Handling 
The website uses a combination of HTML and JavaScript. The page structure itself is rendered in HTML, but much of the dynamic content like the T-bill tables, dropdown filters, and the auction date range calendar is powered by JavaScript.   
A network inspection of MAS’s frontend was conducted by inspecting the site to identify their backend JSON API.  

Through this the following was identified: query parameters to call, and headers to send for scraping.  
 
When working with webscraping, there may be anti-scraping measures enforced to protect against webscraping.
Some typical anti-scraping measures are as below:
  - Rate Limiting
  - CAPTCHAs or Login Walls
  - Frequent HTTP status code errors like 403 Forbidden, 429 Too Many Requests, or 503 Service Unavailable responses suggest rate limiting or blocking.
  - Inspect JavaScript Rendering
  - Analyze Network Traffic
  - Fingerprinting or Bot Detection Services (Cloudflare, Akamai, or Datadome headers often indicate bot protection layers.)

> [!NOTE]
> The API endpoint that we used in this code is part of the official MAS API portal, which is designed specifically for public programmatic access to financial data.

While working with said API, I did not face any anti-scraping measures.  
Still, I've included pacing in the code for respectful usage

In the scenario where there were anti-scraping measures enforced, it is vital to check if there are any legal complications for webscraping which can usually be found in their Terms of Use.    

A quick look at the type of data can also identify if it is potentially illegal (i.e. PDPA data, copyright content)  
If the website passes the legal check and still has anti-scraping measures, here are some examples for an ethical workaround:
- Allowing for time delays
- Using a headless browser
- Use Rotating Proxies
- Randomize Headers and Delays
- Use Session Cookies
- Solve CAPTCHAs (if permitted): Services like 2Captcha or manual intervention can help, but only use them where legally and ethically appropriate.

Running the code and error debugging will help to adjust for the correct workaround to take.  

### Analysis & Summary
While studying the records along with the graph, the below analysis can be derived:  
- Yields ranged from approximately 0.10% during pandemic lows up to 4.19% at peak inflation-driven auctions.
- A clear upward trend was visible from mid-2022 through early 2024, matching global tightening cycles.
- Recent auctions show stabilization around 3.5%–3.8%, suggesting a plateau in interest rates.
- T-bills are auctioned bi-weekly, predominantly with 6-month maturities.
- Some quarters show a higher concentration (e.g., Q4 2022–Q2 2023), aligning with increased government funding activity or rollover schedules.

Data quality is fairly consistent with minimal missing data and excellent consistency.  
All dates were in ISO format (YYYY-MM-DD), which made temporal analysis straightforward.  
Yield values were also numeric and required no transformation beyond type conversion


### Possible Enhancements
- Automating the scrape to a schedule with Task Scheduler
- Adding a GUI for user-selectable filters like date
- Enhance the graph by scraping more fields like auction amount, median yields and average yields

  
 
### The End

<br>.
<br>.
<br>.
<br>.
<br>.
<br>.
<br>.

## Footnotes: Inspecting the website with screenshots

### Sourcing the API
  Navigate to the site and inspect the code  
  Under **Network**, filter by **Fetch/XHR** and go to **Headers** tab  
  Find the Name that shows the Request URL.  
  Reload the page if nothing shows up. 
  The URL up to "?" would serve as the API URL  
    
  The URL also allows us to identify: 
  - Query Parameters (Filters to set: i.e. Date, Product type)
  - Number of rows (To handle pagination if needed)
  - Sorting (The order of results returned)
       
![image](https://github.com/user-attachments/assets/3e47d2b1-1cfa-47cc-b40e-fba25cdbe839)  


## Selecting Headers
  Under **Request Headers**, mirror the metadata notes to include in the Header parameters.  
  Historically, websites used to block non-Netscape browsers. To stay compatible, browsers started including the token “Mozilla” in their User-Agent string  
  The Accept header tells the server which responses are preferred:
  - text/html : browser-style web page
  - application/xml: structured XML data
  - image/png: when requesting an image
  - application/json: Parsing with JSON
  

![image](https://github.com/user-attachments/assets/92e17c76-eb2c-4c06-bb5c-b7d20e2e9a02)

  
## Selecting the columns to retrieve in a for and while loop in json format
Under **Response** tab, I spotted the code that shows the tags of the columns that I require in my data.  
(i.e. Issue Date, Cut-off Yield)  
![image](https://github.com/user-attachments/assets/3aaf6266-9b64-4c20-96ec-311b5c190e4f)

 While tweaking the graph plotting section, I did not want to keep pinging MAS yet still want to keep all code in one python executable.
  So taking this into consideration, a "USE_OFFLINE" section was added.
