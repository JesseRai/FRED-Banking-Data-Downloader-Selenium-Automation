# FRED Banking Data Downloader (Selenium Automation)

This Python script automates the extraction and download of datasets from the Banking section of the Federal Reserve Economic Data (FRED) website. It uses Selenium to navigate the site, list available datasets, prompt the user for a selection, and download the chosen series.

--------------------------------------------------
## Features

- Automated navigation through FRED using Selenium.
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
   - It sets Chrome preferences so downloads happen without prompts.

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
   - download_dataset(driver, wait, chosen_dataset) finds the link matching the chosen dataset text.
   - It clicks the series, waits for the download button, then clicks:
     - The main download button
     - The "download-data" button for the dataset file
   - It sleeps for a short period to allow the file to finish downloading.

6. Cleanup  
   - main() ensures the WebDriver is closed in a finally block, even if an error occurs.

--------------------------------------------------
## Usage

1. Adjust the download path  
   In main(), update:

   download_path = "/Users/jesserai/PycharmProjects/Monk/Projects/Prtfolio/52/3_FED_Selenium/downloads"

   to a valid path on your system if needed.

2. Run the script from the command line:

   python fred_downloader.py

3. Follow the prompt. Example interaction:

   1: Assets: Total Assets
   2: Deposits: Commercial Banks
   3: Loans: Real Estate Loans
   ...
   Please pick a numerical indicator to extract said dataset:

4. Enter the number of the dataset you want. The corresponding file will be downloaded into the downloads directory.

--------------------------------------------------
## Function Summary

- setup_driver(download_path)  
  Sets up Chrome with a custom download directory and returns (driver, wait).

- navigate_to_banking(driver, wait)  
  Navigates from the FRED homepage to the Banking section.

- get_datasets(driver)  
  Scrapes the list of dataset titles from the Banking page and returns them as a list of strings.

- choose_dataset(datasets)  
  Prints the dataset list, prompts for numeric input, and returns the selected dataset name.

- download_dataset(driver, wait, chosen_dataset)  
  Opens the chosen dataset’s page and triggers the download via FRED’s download buttons.

- main()  
  Orchestrates the workflow: sets up the driver, navigates, scrapes, prompts, downloads, and finally closes the browser.

--------------------------------------------------
## Notes and Limitations

- The script assumes FRED’s HTML structure (classes, link texts, and element IDs) remains the same. If the site layout changes, selectors may need updating.
- User input is minimally validated; entering a non-numeric or out-of-range choice will cause an error.
- The time.sleep(10) call is a simple way to wait for download completion; for production use, consider checking the downloads folder until the file appears.

