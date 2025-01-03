
# 5 Strategies to Build Unblockable Web Scrapers in Python

> Writing web scrapers that avoid getting blocked is a challenging yet essential skill for efficient data collection. While no scraper can be entirely unblockable, implementing specific strategies can significantly improve your scraper's performance and longevity. Here's how.

---

## Introduction

Many readers of my [scraping series](http://blog.adnansiddiqi.me/tag/scraping/) often ask how to write scrapers that don’t get blocked. While achieving a 100% unblockable scraper is nearly impossible, you can greatly extend your scraper’s usability with the strategies outlined in this guide.

---

## 1. Use Dynamic User-Agents

The **User-Agent** string informs the server about the browser and operating system used for a web request. Many websites block requests without a valid User-Agent, so setting one is essential.

### How to Implement
In Python, you can use the `requests` library to set a User-Agent header:

```python
headers = { 
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'
}
r = requests.get('https://example.com', headers=headers)
```

For a more realistic approach, use a random User-Agent from a predefined list:

```python
import numpy as np 

def get_random_ua():
    random_ua = ''
    ua_file = 'ua_file.txt'  # File containing a list of User-Agent strings
    try:
        with open(ua_file) as f:
            lines = f.readlines()
        if lines:
            idx = np.random.randint(len(lines))
            random_ua = lines[idx].strip()
    except Exception as e:
        print(f"Error fetching User-Agent: {e}")
    return random_ua

user_agent = get_random_ua()
headers = {'user-agent': user_agent}
r = requests.get('https://example.com', headers=headers)
```

### Where to Find User-Agent Strings
- [Chrome User-Agents](https://udger.com/resources/ua-list/browser-detail?browser=Chrome)
- [Firefox User-Agents](https://udger.com/resources/ua-list/browser-detail?browser=FIREFOX)

---

## 2. Set Referrers

The **Referrer** header specifies the page that linked to the requested resource. For listing or home pages, you can set a general referrer like Google's homepage. For product pages, use backlinks or relevant category pages as referrers.

```python
headers = {
    'user-agent': user_agent,
    'referer': 'https://google.com'
}
```

Using tools like SEMRush, you can gather genuine backlinks for specific pages and randomize them for use as referrers.

---

## 3. Rotate Proxy IPs

To avoid IP bans, use multiple proxy IPs. Websites often block requests based on static IPs or patterns. High-quality proxy providers like [Oxylabs](http://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=70) can help. Ensure the proxies are randomized to avoid detection.

### Implementing Proxies with Requests

```python
proxy_url = "http://proxy_ip:port"
r = requests.get('https://example.com', headers=headers, proxies={'https': proxy_url})
```

### Using Proxies with Selenium
For Selenium, you can configure proxies like this:

```python
proxy = get_random_proxy().strip()
service_args = [
    f'--proxy={proxy}', 
    '--proxy-type=http', 
    '--proxy-auth=user:password'
]
driver = webdriver.PhantomJS(service_args=service_args)
```

To manage proxies effectively:
- Monitor proxy usage frequency.
- Replace blocked proxies regularly.

---

## 4. Mimic Full Request Headers

Some websites check for specific request headers. You can inspect these headers using browser developer tools and mimic them in your requests.

```python
headers = {
    'user-agent': 'Mozilla/5.0',
    'referrer': 'https://google.com',
    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9',
    'Accept-Encoding': 'gzip, deflate, br',
    'Accept-Language': 'en-US,en;q=0.9',
    'Pragma': 'no-cache'
}
```

---

## 5. Add Delays Between Requests

Delays reduce the likelihood of detection. Use random intervals between requests to simulate human browsing behavior:

```python
import time
import numpy as np

delays = [2, 4, 6, 8, 10]
time.sleep(np.random.choice(delays))
```

For higher efficiency, consider parallelizing your requests with multiprocessing. Learn more in my guide on [speeding up scrapers with multiprocessing](http://blog.adnansiddiqi.me/how-to-speed-up-your-python-web-scraper-by-using-multiprocessing/).

---

## Simplify Web Scraping with ScraperAPI

Stop wasting time on proxies and CAPTCHAs! ScraperAPI’s robust API handles millions of web scraping requests seamlessly. Get structured data from Amazon, Google, Walmart, and more.

- **Automatic Proxy Rotation**: Avoid IP bans.
- **CAPTCHA Handling**: No manual interventions required.
- **Headless Browser Support**: Scrape even JavaScript-heavy pages.

Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## Conclusion

While no scraper is completely unblockable, implementing these strategies—randomized User-Agents, proxies, referrers, delays, and complete headers—can significantly enhance your scraper’s performance and reduce the chances of getting blocked.

If you have additional tips or tricks, share them in the comments! For a hassle-free scraping experience, try ScraperAPI and take the complexity out of web scraping.

[Get Started with ScraperAPI](https://www.scraperapi.com/?fp_ref=coupons)
