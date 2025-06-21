## Task Description  
*Extract structured data from a given website, handle potential challenges such 
as pagination, or dynamic content, and present findings clearly.*

**Website of Choice**  
The website of choice is the [Monetary Authority of Singapore](https://www.mas.gov.sg/).  
It covers all auctioned Singapore Government Treasury Bills (T-bills) from 2020 to present.  

As an existing t-bill customer, I've always been interested in looking at the MAS t-bill yields overtime to better support my purchase decisions.  
This along with my finance career of 9 years inspired me to choose MAS Tbills for my webscraping task.


### **Data Extraction**  
The data from MAS is very structured and clean, allowing for easy and clear scraping.  
The goal is to scrape:
- Key Data fields (Auction date and cut off yield)
- Handle pagination (15 rows/page) or load more buttons if applicable
- Extracted over 164 records
  
[Click here](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/T-bill%20web%20scraping%20Python%20Code.md) for the python code.   
To execute the code, copy the code into an IDE and run  
To execute from cmd, save it as a .py file and run the command

### Data Output
For portability and flexibility, I am writing my data into 2 file outputs:
- .csv file so it is compatible with spreadsheet tools
- .json file for easy integration with future python analytics

I've also gone a step further by plotting a graph using matpilotlib for a Yield vs. Date analysis  
This graph visualizes monetary policy shifts and market demand dynamics.  
This is saved as a [.png file](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/t_bill_yields.png) from the code.

### Challenges Handling 
The website uses a combination of HTML and JavaScript. The page structure itself is rendered in HTML, but much of the dynamic content like the T-bill tables, dropdown filters, and the auction date range calendar is powered by JavaScript.   
A network inspection of MAS’s frontend was conducted by inspecting the site to identify their backend JSON API.  
Through this I managed to identify a structured endpoint for auction data, query parameters to call, and headers to send for scraping.  
When considering a website to scrape, there may be measures taken to prevent scraping.  
I define anti-scraping measures as below:
  - Rate Limiting
  - CAPTCHAs or Login Walls
  - Frequent HTTP status code errors like 403 Forbidden, 429 Too Many Requests, or 503 Service Unavailable responses suggest rate limiting or blocking.
  - Inspect JavaScript Rendering
  - Analyze Network Traffic
  - Fingerprinting or Bot Detection Services (Cloudflare, Akamai, or Datadome headers often indicate bot protection layers.)

Fortunately as the official MAS API portal was used, I did not face much anti-scraping measures.

IF HOWEVER I have chosen another website that has anti-scraping measures, I would first investigate if there is any legal complications for webscraping which can be found in their Terms of Use.  
A quick look at the type of data can also identify if it is potentially illegal (i.e. PDPA data, copyright content)  
If the website passes the legal check and still has anti-scraping measures, I'll approach with the following steps (ethically), depending on the situation:  
- Allowing for time delays
- Using a headless browser
- Use Rotating Proxies
- Randomize Headers and Delays
- Use Session Cookies
- Solve CAPTCHAs (if permitted): Services like 2Captcha or manual intervention can help, but only use them where legally and ethically appropriate.

Running the code multiple times and debugging the errors will help to adjust for the correct anti-scraping measures to take.  

### Analysis & Summary
The graph plotting allows for visual analysis of the data drawn.  
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

  
More of my thought process and screenshots can also be found on my [Readme](https://github.com/tessalau/Tessa-s-Mini-Python-Projects/blob/main/Web%20Scraping/Treasury%20Bills%20Statistics%20from%20the%20Monetary%20Authority%20of%20Singapore/README.md)  
### The End
