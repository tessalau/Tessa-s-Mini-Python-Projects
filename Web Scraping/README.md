# SUMMARY
> This python code programmatically extracts, stores, and analyzes Treasury Bill auction results from the Monetary Authority of Singapore.
> Network inspection of MAS’s frontend was conducted using browser DevTools to identify their backend JSON API.
> The scraper is than built with following functions:
> - Requests with user-agent and filter headers
> - Auto pagination
> - Normalized data fields saved to .csv and .jsonformat
> - Graph for yield vs date plotted in using matpilot
>   
> Work process / approach is briefly explained below


> [!NOTE]
> The API endpoint that we used in this code is part of the official [MAS (Monetary Authority of Singapore)](https://www.mas.gov.sg/) API portal, which is designed specifically for public programmatic access to financial data.
>
> 
## Sourcing the API
  Navigate to the site and inspect the code  
  Under **Network**, filter by **Fetch/XHR** and go to **Headers** tab  
  Find the Name that shows you the Request URL.  
  If nothing turns up, reload the page  
  Copy the URL up to "?" which would serve as the API URL  
    
  In that URL, you will also learn: 
  - Query Parameters (Filters to set: i.e. Date, Product type)
  - Number of rows (To handle pagination)
  - Sorting (The order of results returned)
       
![image](https://github.com/user-attachments/assets/3e47d2b1-1cfa-47cc-b40e-fba25cdbe839)  


 
      
  
## Selecting your Headers
  Under **Request Headers**, mirror the metadata notes to include in your Header parameters.  
  Historically, websites used to block non-Netscape browsers. To stay compatible, all major browsers started including the token “Mozilla” in their User-Agent string  
  The Accept header tells the server which responses are preferred:
  - text/html : browser-style web page
  - application/xml: structured XML data
  - image/png: when requesting an image
  - application/json: Parsing with JSON
  

![image](https://github.com/user-attachments/assets/92e17c76-eb2c-4c06-bb5c-b7d20e2e9a02)

  
## Selecting the columns to retrieve in a for and while loop in json format
Under **Response** tab, cycle through the Names until you spot the code that shows you the tags of the columns in your required data.  
Take the tags and loop it in your retrieve code (i.e. Issue Date, Cut-off Yield)  
Put a nice little timer to prevent overwhelming the page. Be responsible!!
![image](https://github.com/user-attachments/assets/3aaf6266-9b64-4c20-96ec-311b5c190e4f)


## Saving the records / code outputs 
For this project, I am writing my data into a:
- .csv file so it is compatible with spreadsheet tools
- .json file for easy integration with future python analytics
- .png image for
  - Yield vs. date graph plotted using the json output file and matpilotlib
  - The graph visualizes monetary policy shifts and market demand dynamics
 
  Now, we may want to tweak the graph plotting section and still keep it all in one python executable.
  However, we don't want to keep pinging MAS to preview changes.
  So taking this into consideration, a "USE_OFFLINE" section was added.


## Enhancements to be considered
- Automating the scrape to a schedule with Task Scheduler
- Adding a GUI for user-selectable filters like date
- Enhance the graph by scraping more fields like auction amount, median yields and average yields


>[!TIP]
> Embed print messages throughout the code for informative progress updates as it runs.
>



>[!CAUTION]
> - Check if you are ALLOWED to scrape data from chosen website by reading their Terms of Use 
> - Be ethical! Are you scraping copyright content or PDPA data?
> - Practice responsible webscraping measures like embedding delays to respect rate limits and avoid excessive requests.
> - Attribute the website if you are publishing or sharing the data
> - Periodically check for updates to the website's API documentation or terms

