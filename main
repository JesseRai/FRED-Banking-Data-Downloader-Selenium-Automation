import os
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.options import Options
from bs4 import BeautifulSoup
import time

def setup_driver(download_path):
    os.makedirs(download_path, exist_ok=True)
    options = Options()
    prefs = {
        "download.default_directory": download_path,
        "download.prompt_for_download": False,
        "download.directory_upgrade": True
    }
    options.add_experimental_option("prefs", prefs)

    driver = webdriver.Chrome(options=options)
    wait = WebDriverWait(driver, 10)
    return driver, wait


def navigate_to_banking(driver, wait):
    url = "https://fred.stlouisfed.org/"
    driver.get(url)

    wait.until(expected_conditions.element_to_be_clickable((By.LINK_TEXT, "Category")))
    driver.find_element(By.LINK_TEXT, "Category").click()

    wait.until(expected_conditions.element_to_be_clickable((By.LINK_TEXT, "Money, Banking, & Finance")))
    driver.find_element(By.LINK_TEXT, "Money, Banking, & Finance").click()

    wait.until(expected_conditions.element_to_be_clickable((By.LINK_TEXT, "Banking")))
    driver.find_element(By.LINK_TEXT, "Banking").click()


def get_datasets(driver):
    soup = BeautifulSoup(driver.page_source, "html.parser")
    table = soup.find(class_="table-responsive")
    trs = table.find_all('tr', class_="series-pager-title")

    datasets = []
    for tr in trs:
        a = tr.find("a")
        if a is None:
            continue
        datasets.append(a.get_text(strip=True))
    return datasets


def choose_dataset(datasets):
    for i, item in enumerate(datasets, start=1):
        print(f"{i}: {item}")
    choice = input("Please pick a numerical indicator to extract said dataset: ")
    return datasets[int(choice) - 1]


def download_dataset(driver, wait, chosen_dataset):
    link = wait.until(
        expected_conditions.element_to_be_clickable((By.LINK_TEXT, chosen_dataset))
    )
    driver.execute_script("arguments[0].click();", link)

    wait.until(expected_conditions.element_to_be_clickable((By.ID, "download-button")))
    driver.find_element(By.ID, "download-button").click()

    wait.until(expected_conditions.element_to_be_clickable((By.ID, "download-data")))
    driver.find_element(By.ID, "download-data").click()

    time.sleep(10)


def main():
    driver = None
    try:
        download_path = "/Users/jesserai/PycharmProjects/Monk/Projects/Prtfolio/52/3_FED_Selenium/downloads"
        driver, wait = setup_driver(download_path)
        navigate_to_banking(driver, wait)
        datasets = get_datasets(driver)
        chosen_dataset = choose_dataset(datasets)
        download_dataset(driver, wait, chosen_dataset)
    finally:
        if driver is not None:
            driver.quit()


if __name__ == "__main__":
    main()
