# Gather-job-listings-data
o analyze job demand versus job supply in specific areas around Jakarta, we can create a Python script that gathers job listings data from various sources (like job boards) and performs a simple analysis. Below is a sample code structure that outlines how you could accomplish this using web scraping with BeautifulSoup and data analysis with pandas.
Step 1: Set Up the Environment

Make sure to install the required libraries:

bash

pip install requests beautifulsoup4 pandas

Step 2: Create the Research Script

This script will fetch job listings from a hypothetical job board and analyze the demand in Jakarta City, Bekasi, and BSD.
2.1 Python Script

python

import requests
from bs4 import BeautifulSoup
import pandas as pd

def fetch_job_data(area):
    # Replace with the actual job listing URL or API endpoint
    url = f'https://example-job-portal.com/jobs?location={area}'
    response = requests.get(url)
    
    if response.status_code != 200:
        print(f"Failed to fetch data for {area}. Status code: {response.status_code}")
        return []

    soup = BeautifulSoup(response.text, 'html.parser')
    
    # Assuming job listings are within <div class="job-listing">
    job_listings = soup.find_all('div', class_='job-listing')
    jobs = []
    
    for job in job_listings:
        title = job.find('h2').text.strip()  # Job title
        company = job.find('h3').text.strip()  # Company name
        jobs.append({'title': title, 'company': company, 'area': area})
    
    return jobs

def analyze_jobs(jobs):
    df = pd.DataFrame(jobs)
    
    # Count job listings by area
    job_count = df['area'].value_counts()
    print("\nJob Demand by Area:")
    print(job_count)

    # Assuming you have a way to get job supply data, for example, static numbers
    job_supply = {
        'Jakarta City': 500,
        'Bekasi': 300,
        'BSD': 200
    }
    
    demand_supply_df = pd.DataFrame({
        'Job Demand': job_count,
        'Job Supply': pd.Series(job_supply)
    })

    # Calculate difference
    demand_supply_df['Difference'] = demand_supply_df['Job Demand'] - demand_supply_df['Job Supply']
    
    print("\nJob Demand vs Supply:")
    print(demand_supply_df)

if __name__ == "__main__":
    areas = ['Jakarta City', 'Bekasi', 'BSD']
    all_jobs = []
    
    for area in areas:
        jobs = fetch_job_data(area)
        all_jobs.extend(jobs)
    
    if all_jobs:
        analyze_jobs(all_jobs)

Explanation of the Code

    Data Fetching: The fetch_job_data function scrapes job listings from a specified URL. Replace the URL with a real job portal that supports scraping.
    HTML Parsing: Using BeautifulSoup, it extracts job titles and company names and stores them in a list.
    Data Analysis:
        The analyze_jobs function converts the job data into a pandas DataFrame.
        It counts the number of jobs per area.
        It compares the demand with a predefined job supply for each area.
        It calculates the difference between demand and supply.
    Execution: The main block of the script iterates through the target areas, collects job data, and performs the analysis.

Step 3: Running the Script

Run the script in your terminal or Python environment. Ensure you have access to the internet to fetch data from the job portal.
Final Notes

    Job Supply Data: The job supply data is currently static. You may want to obtain this from a reliable source or database.
    Real Job Portal: The provided URL is just a placeholder. You’ll need to find a job portal that allows scraping or has an API.
    Error Handling: You might want to include more comprehensive error handling for robustness.
    Rate Limiting: When scraping, be sure to adhere to the site's robots.txt file and rate limit your requests to avoid being blocked.

This framework should help you get started on your research into job demand and supply in the Jakarta area. 
