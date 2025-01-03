
# Asynchronous Web Scraping with Python & AIOHTTP

Web scraping is an essential skill in the modern data-driven world, and Python offers a variety of tools to accomplish this. In this tutorial, we'll explore scraping multiple URLs asynchronously using Python's `aiohttp` module, compare it to the synchronous approach, and highlight why asynchronous scraping can be significantly more efficient.

## Why Asynchronous Web Scraping?

Asynchronous scraping allows you to make multiple web requests concurrently, reducing the time spent waiting for server responses. This can be particularly useful when dealing with large-scale scraping tasks where efficiency matters. Below, we'll demonstrate how to implement both asynchronous and synchronous scraping approaches.

---

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. Start your free trial today! 👉 [ScraperAPI](https://www.scraperapi.com/?fp_ref=coupons)

---

## Getting Started with Asynchronous Scraping

To follow along, you will need the following:
- Python installed on your system.
- The `aiohttp` and `BeautifulSoup` modules installed via pip.
- A CSV file (`urls.csv`) containing a single column labeled `url` with the list of URLs to scrape.

### 1. Setting Up the Main Function

Create a Python file and define the main function. Mark the function as asynchronous to enable the event loop for concurrent execution.

```python
import asyncio

async def main():
    print('Saving the output of extracted information')

loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

### 2. Tracking Execution Time

Monitoring script performance is crucial for optimization. Record the time at the start of the script and calculate the execution time after the process is complete.

```python
import time

async def main():
    start_time = time.time()

    print('Saving the output of extracted information')

    time_difference = time.time() - start_time
    print(f'Scraping time: %.2f seconds.' % time_difference)

loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

### 3. Reading the URLs

Load the URLs from the `urls.csv` file. Use Python's `csv` module to read the file and loop over each URL.

```python
import csv

async def read_urls():
    with open('urls.csv') as file:
        csv_reader = csv.DictReader(file)
        for row in csv_reader:
            print(row['url'])
```

### 4. Creating Asynchronous Scraping Tasks

Using the `aiohttp` module, create tasks for scraping the URLs. Each task will make a request to the URL, parse the HTML with `BeautifulSoup`, and extract the necessary data.

```python
from aiohttp import ClientSession
from bs4 import BeautifulSoup

async def scrape(url, session):
    async with session.get(url) as response:
        html = await response.text()
        soup = BeautifulSoup(html, 'html.parser')

        # Example: Extract book title and product info
        book_name = soup.find('h1').text.strip()
        product_info = {tag.text.strip(): value.text.strip() for tag, value in zip(soup.find_all('th'), soup.find_all('td'))}
        return book_name, product_info

async def main():
    urls = ['https://example.com']  # Replace with URLs from the CSV
    async with ClientSession() as session:
        tasks = [scrape(url, session) for url in urls]
        results = await asyncio.gather(*tasks)
        for book_name, product_info in results:
            print(book_name, product_info)
```

### 5. Saving Scraped Data

Save the scraped data into a JSON file for easy access and future use.

```python
import json

def save_product(book_name, product_info):
    file_name = book_name.replace(' ', '_') + '.json'
    with open(file_name, 'w') as file:
        json.dump(product_info, file)
```

### 6. Running the Script

Run the script and observe the output. The asynchronous scraping process should take significantly less time compared to a synchronous approach.

---

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. Start your free trial today! 👉 [ScraperAPI](https://www.scraperapi.com/?fp_ref=coupons)

---

## Synchronous Web Scraping

For comparison, let's look at how the same task is handled synchronously using Python's `requests` module.

### 1. Setting Up the Main Function

```python
def main():
    print('Saving the output of extracted information')

main()
```

### 2. Tracking Execution Time

```python
import time

def main():
    start_time = time.time()

    print('Saving the output of extracted information')

    time_difference = time.time() - start_time
    print(f'Scraping time: %.2f seconds.' % time_difference)

main()
```

### 3. Reading the URLs

```python
import csv

def main():
    with open('urls.csv') as file:
        csv_reader = csv.DictReader(file)
        for row in csv_reader:
            print(row['url'])
```

### 4. Scraping and Saving Data

Use `requests` to fetch the HTML and `BeautifulSoup` to parse and extract data. Save the data using the same JSON approach as in the asynchronous example.

---

## Comparing Performance

The difference in performance is evident. While the asynchronous script completes tasks in around **3 seconds**, the synchronous approach can take upwards of **16 seconds** for the same workload. This demonstrates the time efficiency and scalability of asynchronous scraping.

![Performance Comparison](https://github.com/oxylabs/asynchronous-web-scraping-python/raw/main/images/speed_comparison.png)

---

For a hassle-free scraping experience, try **ScraperAPI** today. With robust proxy management and CAPTCHA handling, it simplifies scraping at scale. Start your free trial now! 👉 [ScraperAPI](https://www.scraperapi.com/?fp_ref=coupons)
