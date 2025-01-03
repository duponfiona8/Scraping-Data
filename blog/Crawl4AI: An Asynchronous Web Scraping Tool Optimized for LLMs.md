
# Crawl4AI: An Asynchronous Web Scraping Tool Optimized for LLMs

## Introduction to Crawl4AI 🕷️🤖

Crawl4AI is an open-source, LLM-friendly web scraper that simplifies asynchronous web crawling and data extraction for large language models (LLMs) and AI applications. 🆓🌐

For the synchronous version, check out the [README.sync.md](https://github.com/unclecode/crawl4ai/blob/main/README.sync.md). Previous versions can be accessed on branch [V0.2.76](https://github.com/unclecode/crawl4ai/blob/v0.2.76).

### Key Features ✨

- 🆓 Completely free and open source
- 🚀 High performance, outpacing many paid services
- 🤖 LLM-friendly output formats (JSON, cleaned HTML, Markdown)
- 🌍 Support for concurrent crawling of multiple URLs
- 🎨 Extracts all media tags (images, audio, video)
- 🔗 Extracts all external and internal links
- 📚 Metadata extraction
- 🔄 Pre-crawl hooks for authentication, headers, and page modifications
- 🕵️ Customizable user agents
- 🖼️ Page screenshots
- 📜 Custom JavaScript execution before crawling
- 📊 Structured data extraction with LLMs using `JsonCssExtractionStrategy`
- 📚 Flexible chunking strategies: topic-based, regex, sentence-based, etc.
- 🧠 Advanced extraction strategies: cosine clustering, LLMs, and more
- 🎯 Precise data extraction via CSS selectors
- 📝 Pass instructions/keywords for refined extractions
- 🔒 Proxy support for privacy and access
- 🔄 Session management for complex multi-page crawls
- 🌐 Asynchronous architecture for enhanced performance and scalability

---

## Installation 🛠️

Crawl4AI offers flexible installation options to suit a variety of use cases. You can install it as a Python package or use Docker.

### 1. Using `pip` 🐍

Choose the installation option that best fits your needs:

#### 1.1 Basic Installation

For basic web crawling and scraping tasks:

```bash
pip install crawl4ai
```

This installs Crawl4AI's asynchronous version, leveraging Playwright for web scraping.

> **Note**: The installation script should automatically set up Playwright. If you encounter any Playwright-related errors, install it manually using one of the following commands:

```bash
playwright install
```

```bash
python -m playwright install chromium
```

#### 1.2 Installing the Synchronous Version

Coming soon: Use the synchronous version for tasks where asynchronous crawling is unnecessary.

#### 1.3 Development Installation

Use this mode if you're contributing to the Crawl4AI codebase.

---

### 2. Using Docker 🐳

Docker images are being created and will be pushed to Docker Hub, offering an easy way to run Crawl4AI in a containerized environment. Stay tuned for updates!

---

## Advanced Usage 🔬

### Using Proxies

Crawl4AI supports proxy configurations to enhance privacy and access restricted content.

---

### Extracting Structured Data Without LLMs

Here’s an example of using Crawl4AI to extract structured data:

```python
import os
import asyncio
from crawl4ai import AsyncWebCrawler
from crawl4ai.extraction_strategy import LLMExtractionStrategy
from pydantic import BaseModel, Field

class OpenAIModelFee(BaseModel):
    model_name: str = Field(..., description="Name of the OpenAI model.")
    input_fee: str = Field(..., description="Fee for input token for the OpenAI model.")
    output_fee: str = Field(..., description="Fee for output token for the OpenAI model.")

async def main():
    async with AsyncWebCrawler(verbose=True) as crawler:
        result = await crawler.arun(
            url='https://openai.com/api/pricing/',
            word_count_threshold=1,
            extraction_strategy=LLMExtractionStrategy(
                provider="openai/gpt-4o", api_token=os.getenv('OPENAI_API_KEY'), 
                schema=OpenAIModelFee.schema(),
                extraction_type="schema",
                instruction="""From the crawled content, extract all mentioned model names along with their fees for input and output tokens. 
                Do not miss any models in the entire content. One extracted model JSON format should look like this: 
                {"model_name": "GPT-4", "input_fee": "US$10.00 / 1M tokens", "output_fee": "US$30.00 / 1M tokens"}."""
            ),            
            bypass_cache=True,
        )
        print(result.extracted_content)

if __name__ == "__main__":
    asyncio.run(main())
```

---

### Session Management and Crawling Dynamic Content

Crawl4AI excels in handling complex scenarios, such as crawling dynamically loaded content using JavaScript. Here's an example of scraping multiple GitHub commit pages:

```python
import asyncio
import re
from bs4 import BeautifulSoup
from crawl4ai import AsyncWebCrawler

async def crawl_typescript_commits():
    first_commit = ""
    async def on_execution_started(page):
        nonlocal first_commit 
        try:
            while True:
                await page.wait_for_selector('li.Box-sc-g0xbh4-0 h4')
                commit = await page.query_selector('li.Box-sc-g0xbh4-0 h4')
                commit = await commit.evaluate('(element) => element.textContent')
                commit = re.sub(r'\s+', '', commit)
                if commit and commit != first_commit:
                    first_commit = commit
                    break
                await asyncio.sleep(0.5)
        except Exception as e:
            print(f"Warning: New content didn't appear after JavaScript execution: {e}")

    async with AsyncWebCrawler(verbose=True) as crawler:
        crawler.crawler_strategy.set_hook('on_execution_started', on_execution_started)

        url = "https://github.com/microsoft/TypeScript/commits/main"
        session_id = "typescript_commits_session"
        all_commits = []

        js_next_page = """
        const button = document.querySelector('a[data-testid="pagination-next-button"]');
        if (button) button.click();
        """

        for page in range(3):  # Crawl 3 pages
            result = await crawler.arun(
                url=url,
                session_id=session_id,
                css_selector="li.Box-sc-g0xbh4-0",
                js=js_next_page if page > 0 else None,
                bypass_cache=True,
                js_only=page > 0
            )

            assert result.success, f"Failed to crawl page {page + 1}"

            soup = BeautifulSoup(result.cleaned_html, 'html.parser')
            commits = soup.select("li")
            all_commits.extend(commits)

            print(f"Page {page + 1}: Found {len(commits)} commits")

        await crawler.crawler_strategy.kill_session(session_id)
        print(f"Successfully crawled {len(all_commits)} commits across 3 pages")

if __name__ == "__main__":
    asyncio.run(crawl_typescript_commits())
```

---

## Performance Comparison 🚀

Crawl4AI is designed with speed as a primary focus. Our tests show that it significantly outperforms paid services like Firecrawl:

- **Simple Crawls**: Crawl4AI is over 4x faster than Firecrawl.
- **JavaScript Execution**: Even with JavaScript execution to load more content, Crawl4AI remains faster than Firecrawl's simple crawls.

Full comparison code can be found in the repository under `docs/examples/crawl4ai_vs_firecrawl.py`.

---

## Special Offer 🎉

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. Start your free trial today! 👉 [https://www.scraperapi.com/?fp_ref=coupons](https://www.scraperapi.com/?fp_ref=coupons)
