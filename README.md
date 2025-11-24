# FRED Banking Data Downloader (Selenium Automation)

This Python script automates the extraction and download of datasets from the Banking section of the Federal Reserve Economic Data (FRED) website. It uses Selenium to navigate the site (in full or headless mode), list available datasets, prompt the user for a selection, and download the chosen series.

--------------------------------------------------
## Features

- Automated navigation through FRED using Selenium.
- Runs optionally in **headless mode** (Chrome without a visible window).
- Scrapes all available Banking datasets using BeautifulSoup.
- Displays datasets with numbered options in the terminal.
- Prompts the user to choose a dataset by number.
- Downloads the selected dataset into a specified local folder.
- Modular, function-based structure for clarity and reuse.

--------------------------------------------------
## Requirements

Python packages:
- selenium
- beautifulsoup4

Install via:
- pip install selenium beautifulsoup4

You also need:
- Google Chrome installed
- A compatible ChromeDriver accessible on your system (or managed by Selenium 4)

--------------------------------------------------
## Project Structure

Example layout:

your_project/
    fred_downloader.py        # This script
    downloads/                # Folder where FRED files are saved

The script will create the downloads directory if it does not exist.

--------------------------------------------------
## How It Works

1. Driver Setup  
   - setup_driver(download_path) creates a Chrome WebDriver configured to save files into download_path.
   - It configures Chrome download preferences to avoid prompts.
   - It adds the argument:
       --headless=new  
     so Chrome can run without opening a visible window.

2. Navigation  
   - navigate_to_banking(driver, wait) opens https://fred.stlouisfed.org/ and clicks through:
     - "Category"
     - "Money, Banking, & Finance"
     - "Banking"

3. Dataset Extraction  
   - get_datasets(driver) parses the Banking page using BeautifulSoup.
   - It looks for rows with class "series-pager-title" inside the table with class "table-responsive".
   - It returns a Python list of dataset names.

4. User Selection  
   - choose_dataset(datasets) prints each dataset with a numeric index (1, 2, 3, ...).
   - It prompts you to enter the number of the dataset you want.
   - It returns the chosen dataset name.

5. Download  
   - download_dataset(driver, wait, chosen_dataset) opens the chosen dataset page.
   - It waits for the download buttons and clicks:
     - The main download button
     - The "download-data" button for the dataset file
   - It waits briefly to ensure the file finishes downloading.

6. Cleanup  
   - main() ensures the WebDriver is closed in a finally block, even if an error occurs.

--------------------------------------------------
## Usage

1. Adjust the download path  
   In main(), update:

   download_path = "/Users/jesserai/PycharmProjects/Monk/Projects/Prtfolio/52/3_FED_Selenium/downloads"

   if needed.

2. Run the script:

   python fred_downloader.py

3. Follow the on-screen prompt. Example:

   1: Assets: Total Assets  
   2: Deposits: Commercial Banks  
   3: Loans: Real Estate Loans  
   ...  
   Please pick a numerical indicator to extract said dataset:

4. Enter the number of the dataset you want.  
   The file will download into the downloads directory while Chrome runs headlessly.

--------------------------------------------------
## Function Summary

- setup_driver(download_path)  
  Sets up Chrome in headless mode with a specified download directory. Returns (driver, wait).

- navigate_to_banking(driver, wait)  
  Navigates from the FRED homepage to the Banking section.

- get_datasets(driver)  
  Scrapes and returns the dataset names.

- choose_dataset(datasets)  
  Displays the numbered dataset list and returns the user’s selection.

- download_dataset(driver, wait, chosen_dataset)  
  Opens the dataset page and triggers the Excel download.

- main()  
  Orchestrates all steps and safely closes the browser.

--------------------------------------------------
## Notes and Limitations

- Some websites behave differently in headless mode; if FRED changes visibility rules or layout, selectors may need updating.
- time.sleep(10) is a simple wait; a more robust approach would monitor the downloads directory for file completion.


