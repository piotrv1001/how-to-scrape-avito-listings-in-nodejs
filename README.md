# How to Scrape Avito Listings in Node.js

This example shows how to scrape Avito category listings and optional seller details in Node.js using the [Avito Listings Scraper](https://apify.com/piotrv1001/avito-listings-scraper) Actor on Apify. It calls an existing Actor rather than implementing an Avito scraper.

## What this example does

- Calls `piotrv1001/avito-listings-scraper`
- Passes an Avito category URL, global item limit, and detail-mode setting
- Waits for the Actor run to finish
- Fetches enriched and listing-only fallback rows from the dataset
- Prints each structured result

## Prerequisites

- [Node.js](https://nodejs.org) 18 or newer
- An [Apify account](https://console.apify.com/sign-up)
- An Apify API token from **Settings → Integrations**

## Installation

```bash
npm install
```

## Environment setup

```bash
cp .env.example .env
```

Add your token to `.env`:

```env
APIFY_TOKEN=your_apify_token_here
```

## Usage

```bash
npm start
```

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    startUrls: [
        { url: 'https://www.avito.ru/novosibirsk/detskaya_odezhda_i_obuv' },
    ],
    maxItems: 30,
    scrapeDetails: true,
    proxyConfiguration: {
        useApifyProxy: true,
    },
};

// Run the Actor and wait for it to finish
const run = await client.actor('piotrv1001/avito-listings-scraper').call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

[`sample-output.json`](./sample-output.json) contains one enriched listing and one listing-only fallback. Key fields include the listing ID and URL, numeric and displayed prices, city and location, seller context, description, category-specific parameters, photos, and the `detailsAvailable` fallback flag.

## Use cases

- Compare asking prices within one city and category
- Monitor category inventory across scheduled snapshots
- Analyze category-specific product or property attributes
- Separate private and business sellers where the source provides the type
- Build a shortlist with descriptions and all public listing photos

## Try the Actor on Apify

**[Open the Avito Listings Scraper on Apify](https://apify.com/piotrv1001/avito-listings-scraper)**

## Related resources

- [How to Scrape Avito Listings, Prices, and Seller Details](https://www.falconscrape.com/blog/how-to-scrape-avito-listings)
- [Apify JavaScript client documentation](https://docs.apify.com/api/client/js/docs)

## License

MIT
