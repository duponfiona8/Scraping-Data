# How to Use Google Sheets to Scrape Amazon and Walmart Product Data

## Introduction

Recently, I came across an incredible video while browsing Twitter:

<figure>
<iframe src="https://streamable.com/e/2vsv8u?autoplay=1&amp;nocontrols=1"></iframe>
</figure>

The video demonstrated how you could input a product link from Amazon into Excel and directly retrieve product details such as title, price, and more. Essentially, it acts as an Amazon scraper, allowing you to fetch product information to your spreadsheet by simply knowing the ASIN.

I decided to test it out, and here's what I found. First, you'll need to use [Google Sheets](https://docs.google.com/spreadsheets/) and install the [ImportFromWeb](https://workspace.google.com/marketplace/app/importfromweb_web_scraping_in_google_she/278587576794) plugin. Without this plugin, you won’t have access to the required functions. 

### Getting Started
- **Product Link Formula:**  
  Combine `https://www.amazon.com/dp/` with the product's ASIN to create the product link.  
- **Plugin Installation:**  
  Install the ImportFromWeb plugin to enable scraping functionalities.  

Once everything is set up, it works just like the video showed. Sometimes the data takes a moment to load, but it retrieves the product details accurately.

> Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## Features of the ImportFromWeb Plugin

### Amazon Product Page: Supported Parameters
The ImportFromWeb plugin allows you to scrape detailed information from Amazon product pages. Below is a comprehensive list of parameters supported:

#### General Information
- **asin:** Amazon Standard Identification Number (ASIN)  
- **title:** Product name  
- **url:** Product URL  
- **brand_name:** Product’s brand  
- **manufacturer:** Manufacturer’s name  
- **model:** Item model number  
- **country_of_origin:** Product’s country of origin  
- **categories:** Categories the product fits in (breadcrumb)  
- **categories_links:** URLs of the categories  
- **best_seller_main_category:** Best seller rank category  
- **best_seller_main_rank:** Best seller rank  
- **rating:** Average product rating  
- **times_evaluated:** Number of reviews  

#### Product Description
- **a_plus_content:** Manufacturer’s description  
- **bullet_point_X:** Specific bullet point (replace X with the number)  
- **bullet_points:** All description bullet points  
- **description:** Product description  

#### Media
- **featured_image_source:** Main product image  
- **image_X_source:** Additional images (replace X with the number)  
- **other_images_sources:** All product images  
- **has_video:** Indicates if the page has a video (true/false)  

#### Pricing and Offers
- **availability:** Stock availability  
- **list_price:** Manufacturer’s suggested retail price  
- **sale_price:** Sale price  
- **sale_price_per_unit:** Unit sale price  
- **buybox_quantity_max:** Maximum selectable quantity  
- **buybox_winner:** Buybox seller  
- **buybox_winner_link:** URL of the Buybox seller's page  
- **vendors_names:** Alternative sellers  
- **vendors_prices:** Alternative sellers’ prices  
- **vendors_links:** URLs of alternative sellers’ pages  

#### Product Variations
- **current_variation_headers:** Headers of possible variations  
- **variation_X_name:** Name of variation (replace X with the number)  
- **variation_X_child_images_source:** Images of the selected variation  
- **variation_X_child_texts:** Values of the selected variation  

#### Technical Characteristics
- **feature_headers:** Feature headers  
- **feature_values:** Feature values  
- **capacity:** Product capacity  
- **color_name:** Product color  
- **style_name:** Product style  
- **Has_climate_pledge_friendly_badge:** Climate pledge-friendly certification  

---

## Amazon Search Page: Supported Parameters
The plugin also supports scraping data from Amazon search pages. Here’s what you can extract:

- **asin:** ASIN for each product  
- **title:** Title of each product  
- **price:** Price of each product  
- **rating:** Average rating  
- **reviews:** Number of reviews  
- **link:** Product URL  
- **featured_image_source:** Main image source  

> Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)

---

## Conclusion
Using Google Sheets and the ImportFromWeb plugin makes it simple to extract data from Amazon and Walmart product pages. With a few clicks, you can fetch detailed information to streamline your workflow. 

If you’re working on large-scale scraping projects, consider using [ScraperAPI](https://www.scraperapi.com/?fp_ref=coupons) for efficient and scalable solutions.
