# Pandas in Python takes a CSV file and creates a DataFrame, which is a
# two-dimensional, size-mutable, and potentially heterogeneous tabular data structure
# with labeled axes (rows and columns).
# It is similar to a spreadsheet or SQL table. The read_csv() function in pandas is used to
# read data from a CSV
# file and load it into a DataFrame.
import pandas as pd
# In Python, the re module provides regular expression operations.
# To use it, it must be imported using the import statement:
import re
# The time module in Python provides functions for working with time-related tasks.
# It can be imported using the statement import time. This makes its functions and classes available for use in your code.
import time
# This function in combination with pd allows you to read in your CSV file
import csv
# The json module in Python is used for working with JSON (JavaScript Object Notation) data.
# It allows you to convert Python objects into JSON strings (serialization) and JSON strings back into Python objects (deserialization)
import json
# This module defines a standard interface to break Uniform Resource Locator (URL) strings up in components
# (addressing scheme, network location, path etc.), to combine the components back into a URL string,
# and to convert a “relative URL” to an absolute URL given a “base URL.”
from urllib.parse import urlparse
# Beautiful Soup is a library that makes it easy to scrape information from web pages.
# It sits atop an HTML or XML parser, providing Pythonic idioms for iterating, searching, and modifying the parse tree.
from bs4 import BeautifulSoup
# Selenium is an open-source automation testing tool that supports various scripting languages such as
# C#, Java, Perl, Ruby, JavaScript, and others.
# The choice of scripting language can be made based on the specific requirements of the application being tested.
from selenium import webdriver
# A ChromeDriver is a separate executable or a standalone server that Selenium WebDriver uses to launch Google Chrome.
# Here, a WebDriver refers to a collection of APIs used to automate the testing of web applications.
from selenium.webdriver.chrome.options import Options
# The selenium.webdriver.chrome.service.Service class in Selenium is responsible for managing the ChromeDriver process,
# allowing for more control over the ChromeDriver's execution,
# such as specifying the executable path, port, arguments, and logging behavior.
from selenium.webdriver.chrome.service import Service
# The selenium.webdriver.common.by import By statement imports the By class from the Selenium WebDriver library.
# The By class provides a set of constants that are used to specify how to locate elements on a web page,
# such as by their ID, class name, XPath, link text, or partial link text.
from selenium.webdriver.common.by import By
# WebDriverWait in Selenium is an explicit wait mechanism that pauses script execution until a specific condition is met,
# or a timeout is reached. It allows you to instruct the WebDriver to wait for a particular element to appear,
# become clickable, or a certain page state to be reached before proceeding.
# This is crucial for handling dynamic web pages where elements load asynchronously or if a website wants to protect
# against webpage scraping; your script acts more human if it takes its time
from selenium.webdriver.support.ui import WebDriverWait
# The line from selenium.webdriver.support import expected_conditions as EC imports a module containing a set of
# pre-defined expected conditions for use with Selenium's WebDriverWait.
# This allows you to wait for specific conditions to be met on a web page before proceeding with your automated actions.
# By importing these conditions under the alias EC, you can easily call them within your WebDriverWait's until() method.
from selenium.webdriver.support import expected_conditions as EC
# The code from selenium.common.exceptions import TimeoutException, WebDriverException imports two specific exception
# classes from the Selenium library. TimeoutException is raised when Selenium waits for an element or condition to be met,
# but the specified timeout period is exceeded. WebDriverException is a more general exception that can occur during
# WebDriver interactions, often indicating issues with the WebDriver's communication with the browser.
from selenium.common.exceptions import TimeoutException, WebDriverException
# The random module in Python is a built-in module that implements pseudo-random number generators. It can be used in
# various applications, such as simulations, games, and security. To use the random module, it must first be imported
import random

# List of user agents to rotate through
USER_AGENTS = [
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36',
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:89.0) Gecko/20100101 Firefox/89.0',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.1.1 Safari/605.1.15',
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36 Edg/91.0.864.59',
    'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.107 Safari/537.36'
]

# Common job title patterns
JOB_TITLE_PATTERNS = [
    r'Director\s+of\s+[\w\s]+',
    r'[\w\s]+(Director)[\w\s]*',
    r'(Senior|Principal|Lead|Head)\s+[\w\s]+(Scientist|Researcher|Manager)',
    r'[\w\s]+(Pharmacology|Oncology|Discovery)[\w\s]+(Scientist|Researcher|Lead|Head|Manager)',
    r'[\w\s]+\bPhD\b[\w\s]+',
    r'(Research|Science|Discovery)\s+[\w\s]+(Lead|Manager|Director)',
    r'[\w\s]+(Scientist|Manager|Director)[\w\s]*(I{1,3}|V?I{0,3})[\W]*$'
]


# Function to set up a Selenium webdriver
def setup_driver():
    chrome_options = Options()
    chrome_options.add_argument("--headless")  # Run in headless mode (no GUI)
    chrome_options.add_argument("--disable-gpu")
    chrome_options.add_argument("--no-sandbox")
    chrome_options.add_argument("--disable-dev-shm-usage")
    chrome_options.add_argument(f"user-agent={random.choice(USER_AGENTS)}")
    chrome_options.add_argument("--window-size=1920,1080")

    # Create a new webdriver
    driver = webdriver.Chrome(options=chrome_options)
    return driver


# Function to extract job titles from page text
def extract_job_titles(text):
    titles = set()
    for pattern in JOB_TITLE_PATTERNS:
        matches = re.finditer(pattern, text, re.IGNORECASE)
        for match in matches:
            title = match.group(0).strip()
            # Filter out titles that are likely not actual job titles (too short or too long)
            if 5 < len(title) < 100:
                titles.add(title)
    return list(titles)


# Function to get context around a keyword match
def get_context(text, keyword, window_size=100):
    # Create a pattern that matches the keyword with word boundaries
    pattern = r'\b' + re.escape(keyword.lower()) + r'\b'

    # Find all matches
    contexts = []
    for match in re.finditer(pattern, text.lower()):
        start = max(0, match.start() - window_size)
        end = min(len(text), match.end() + window_size)

        # Get the context
        context = text[start:end].strip()

        # Add ellipses if the context is cut
        if start > 0:
            context = "..." + context
        if end < len(text):
            context = context + "..."

        contexts.append(context)

    return contexts


# Function to scrape a job listing page using Selenium
def scrape_job_page(company_name, url, keywords):
    print(f"Scraping {company_name} at {url}...")

    # Dictionary to store results
    results = {
        'company': company_name,
        'url': url,
        'job_titles': [],
        'keyword_matches': {},
        'status': 'Success'
    }

    driver = None
    try:
        # Set up the driver
        driver = setup_driver()

        # Add a random delay between 3-7 seconds
        time.sleep(random.uniform(3, 7))

        # Navigate to the URL
        driver.get(url)

        # Wait for the page to load (wait for body element)
        try:
            WebDriverWait(driver, 15).until(
                EC.presence_of_element_located((By.TAG_NAME, "body"))
            )
        except TimeoutException:
            results['status'] = "Error: Page load timeout"
            return results

        # Additional random wait to simulate human behavior
        time.sleep(random.uniform(2, 5))

        # Get the page source and create a BeautifulSoup object
        soup = BeautifulSoup(driver.page_source, 'html.parser')

        # Get all text from the page
        page_text = soup.get_text()
        page_text_lower = page_text.lower()

        # Extract potential job titles
        results['job_titles'] = extract_job_titles(page_text)

        # Search for keywords in the page text and collect context
        for keyword in keywords:
            keyword_lower = keyword.lower()
            if keyword_lower in page_text_lower:
                # Count occurrences
                count = len(re.findall(r'\b' + re.escape(keyword_lower) + r'\b', page_text_lower))

                # Get context for each occurrence
                contexts = get_context(page_text, keyword)

                # Store the results
                results['keyword_matches'][keyword] = {
                    'count': count,
                    'contexts': contexts[:5]  # Limit to first 5 contexts to keep output manageable
                }

        # If no matches were found
        if not results['keyword_matches']:
            results['status'] = 'No keyword matches found'

    except WebDriverException as e:
        results['status'] = f"WebDriver Error: {str(e)}"
    except Exception as e:
        results['status'] = f"Error: {str(e)}"
    finally:
        # Always close the driver
        if driver:
            driver.quit()

    return results


# Main function
def main():
    # Define keywords to search for
    keywords = ['Director', 'Pharmacology', 'Discovery', 'Oncology']

    # Read the CSV file
    df = pd.read_csv('job_searches_company_list_v3.csv')

    # Check if the necessary columns exist
    if 'Companies' not in df.columns or 'Website link to jobs' not in df.columns:
        print("Required columns 'Companies' or 'Website link to jobs' not found in CSV.")
        return

    # Clean the data - remove rows with missing values in key columns
    df = df.dropna(subset=['Companies', 'Website link to jobs'])

    # Process all companies in the CSV
    test_df = df  # Process all companies in the CSV

    # Prepare results list
    all_results = []

    print(f"Starting to scrape {len(test_df)} companies for keywords: {', '.join(keywords)}")
    print("This may take some time as we're using Selenium with random delays to avoid being blocked...")

    # Process each company
    for index, row in test_df.iterrows():
        company_name = row['Companies']
        job_url = row['Website link to jobs']

        # Check if URL is valid
        if not job_url or pd.isna(job_url) or not isinstance(job_url, str):
            print(f"Skipping {company_name}: Invalid URL")
            continue

        # Get the base domain for reporting
        domain = urlparse(job_url).netloc

        # Scrape the job page
        result = scrape_job_page(company_name, job_url, keywords)
        all_results.append(result)

        # Add a longer random delay between companies to be more human-like
        delay = random.uniform(5, 10)
        print(f"Waiting {delay:.1f} seconds before processing the next company...")
        time.sleep(delay)

    # Print results in a more structured way
    print("\n--- DETAILED RESULTS ---")
    for result in all_results:
        print(f"\n{'=' * 50}")
        print(f"COMPANY: {result['company']}")
        print(f"URL: {result['url']}")
        print(f"STATUS: {result['status']}")

        if result['job_titles']:
            print("\nPOTENTIAL JOB TITLES FOUND:")
            for i, title in enumerate(result['job_titles'][:10], 1):  # Show only up to 10 titles
                print(f"  {i}. {title}")
            if len(result['job_titles']) > 10:
                print(f"  ...and {len(result['job_titles']) - 10} more")
        else:
            print("\nNo job titles found.")

        if result.get('keyword_matches'):
            print("\nKEYWORD MATCHES:")
            for keyword, data in result['keyword_matches'].items():
                print(f"\n  {keyword}: {data['count']} occurrences")
                if data['contexts']:
                    print("  Contexts:")
                    for i, context in enumerate(data['contexts'], 1):
                        # Format the context to highlight the keyword
                        highlighted = re.sub(
                            r'\b' + re.escape(keyword) + r'\b',
                            f"**{keyword}**",
                            context,
                            flags=re.IGNORECASE
                        )
                        print(f"    {i}. {highlighted}")
        else:
            print("\nNo keyword matches found.")

    # Save results to JSON for richer data preservation
    with open('job_search_results.json', 'w', encoding='utf-8') as f:
        json.dump(all_results, f, indent=2, ensure_ascii=False)

    # Also save a simplified CSV version for easy viewing
    with open('job_search_results.csv', 'w', newline='', encoding='utf-8') as f:
        writer = csv.writer(f)
        writer.writerow(['Company', 'URL', 'Status', 'Job Titles Found', 'Keyword Match Summary'])

        for result in all_results:
            # Create a summary of job titles (limited to first 5)
            job_titles_summary = '; '.join(result['job_titles'][:5])
            if len(result['job_titles']) > 5:
                job_titles_summary += f"; +{len(result['job_titles']) - 5} more"

            # Create a summary of keyword matches
            keyword_summary = []
            for keyword, data in result.get('keyword_matches', {}).items():
                keyword_summary.append(f"{keyword}: {data['count']}")

            writer.writerow([
                result['company'],
                result['url'],
                result['status'],
                job_titles_summary,
                '; '.join(keyword_summary) if keyword_summary else 'None'
            ])

    print(f"\n{'=' * 50}")
    print("\nResults saved to:")
    print("1. job_search_results.json (detailed data)")
    print("2. job_search_results.csv (simplified summary)")


if __name__ == "__main__":
    main()
