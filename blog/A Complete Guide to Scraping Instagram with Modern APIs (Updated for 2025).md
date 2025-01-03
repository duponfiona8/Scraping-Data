
# A Complete Guide to Scraping Instagram with Modern APIs (Updated for 2025)

## Scraping Instagram Made Easy with ScraperAPI

This blog post is your ultimate guide to scraping public Instagram profiles and posts effectively, using the **ScraperAPI**. By the end of this tutorial, you’ll know how to fetch, parse, and process Instagram data like a pro.

In this example, we’ll scrape posts from a public profile that lists old houses for sale to find the best deals. The detailed scraping logic and code snippets provided ensure a smooth experience.

### Why ScraperAPI Is the Best for Instagram Scraping

Stop wasting time on proxies and CAPTCHAs! **ScraperAPI** simplifies web scraping with its robust API, handling millions of requests effortlessly. Get structured data from platforms like Amazon, Google, Walmart, and Instagram with ease. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## Setting Up the Tools You’ll Need

We’ve created a Python notebook to accompany this tutorial. You can find it in our GitHub repository: [Instagram Scraping Guide](https://github.com/mateuszbuda/instagram-scraping-fish). 

To run the notebook, you’ll need an API key for **ScraperAPI**. Use the discount code **SCRAPE9837861** for easy access. Without it, you risk being blocked by Instagram almost instantly.

---

## Key Considerations for Instagram Scraping

Instagram frequently updates its private (undocumented) API, which we rely on for scraping. This guide was last updated in **February 2024**, so if you encounter issues, it’s likely due to API changes. Feel free to open an issue on the GitHub repository linked above.

---

## Scraping Use Case: Extracting Property Listings

As a practical example, we’ll scrape posts from the public Instagram profile [Stare Domy](https://www.instagram.com/staredomynasprzedaz/) 🏚, which aggregates listings of old houses for sale in Poland. Each post description contains structured data such as:

- **Location** (address, province)
- **Price**
- **House size** in square meters
- **Plot area**

---

## Step 1: Fetch Profile Data via Instagram API

The first step in our scraping process involves calling the following endpoint:

```
https://i.instagram.com/api/v1/users/web_profile_info/?username=staredomynasprzedaz
```

Include this custom header to avoid being blocked:

```
"x-ig-app-id": "936619743392459"
```

The response contains:

- A **user identifier** for further requests
- General profile information
- The first page of posts
- A **next page cursor** for pagination

---

## Step 2: Paginate Using the Instagram GraphQL API

To retrieve subsequent pages of posts, use the Instagram GraphQL API endpoint:

```
https://instagram.com/graphql/query/?query_id=17888483320059182&id={user_id}&first=24&after={end_cursor}
```

### Key Parameters
- `query_id`: Fixed to `17888483320059182`
- `first`: Number of posts per page (default: 24; increase cautiously to avoid detection)
- `after`: Cursor from the previous page response

---

## Step 3: Parse JSON Responses for Post Data

The JSON responses from both endpoints contain similar structures but differ slightly at the top level:

- **Profile response**: Key is `graphql`
- **GraphQL query**: Key is `data`

Our function retrieves the following post details:

- **Shortcode**: URL of the post (`https://www.instagram.com/p/<shortcode>/`)
- **Image URL** 🏞
- **Description** 📝
- **Number of comments** 💬
- **Number of likes** 👍
- **Timestamp** ⏰

---

## Step 4: Complete Instagram Scraping Logic

Once you have all the pieces, you can implement the full scraping logic. Using ScraperAPI, scraping a profile with 300 posts takes approximately **25 seconds**, with each page retrieved in about 2 seconds.

Here’s an example workflow:

1. Call the profile endpoint for initial data and cursor.
2. Paginate through posts using the GraphQL API.
3. Parse JSON responses and store the data in a structured format.

You can easily convert the data into a **Pandas DataFrame** for further processing.

---

## Advanced Parsing: Extract Property Features

From the post descriptions, we can extract detailed property information such as:

- **Location** (address and province) 📍
- **Price** (in PLN) 💰
- **House size** (m²) 🏠
- **Plot area** (m²) 📐

Using regular expressions, you can parse structured text from descriptions and compute derived features like **price per m²**.

For detailed parsing logic, check out the accompanying notebook on GitHub: [Instagram Tutorial Notebook](https://github.com/mateuszbuda/instagram-scraping-fish/blob/master/instagram-tutorial.ipynb).

---

## Data Exploration and Filtering

With the parsed data, you can analyze trends and filter for properties of interest. For example, to find affordable houses:

- **Price**: Below 200,000 PLN
- **Size**: Between 100 m² and 200 m²

Here’s an example post: [Affordable House Example](https://www.instagram.com/p/CYv93e8Nvwh/)

---

## Conclusion: Unlock the Power of ScraperAPI

Scraping public data from Instagram is straightforward with the right tools. **ScraperAPI** simplifies the process, enabling you to scrape even complex websites effortlessly.

👉 Start your free trial today: [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

For any custom use cases or assistance, feel free to reach out via our [contact form](https://www.scraperapi.com/contact).

---
