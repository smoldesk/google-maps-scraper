# Google Maps Scraper

![Google Maps Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/google-maps/banner.webp)

Scrape Google Maps places with phone, website, rating, review count, opening hours and coordinates, plus emails and social profiles from each business website. No login, no browser. Runs on [Apify](https://apify.com/smoldesk/google-maps-scraper).

## What you get

- **Places**: name, categories, address, neighborhood, city, country, latitude/longitude, place ID, CID and Maps link.
- **Contact basics**: phone (local and international) and website, straight from Google Maps.
- **Ratings**: rating, review count, owner-claimed flag, opening hours, timezone, open/closed status.
- **Emails & social profiles** (optional): emails, Facebook, Instagram, LinkedIn, X, TikTok, YouTube and WhatsApp links from the business website. Charged only when something is found.
- **Top reviews** (optional): the up to 5 reviews Google Maps shows without signing in.
- **Lead filters**: minimum rating or reviews, only with a phone, only with or **without a website**, category words, skip closed places.
- **Monitoring**: `onlyNewPlaces` saves only places that weren't there in earlier runs.
- **More than 120 per search**: `splitArea` searches a grid of smaller areas.

## Run it

Open the [Google Maps Scraper on Apify](https://apify.com/smoldesk/google-maps-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~google-maps-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "searchTerms": ["dentist"],
    "locations": ["Austin, TX"],
    "maxPlacesPerSearch": 50,
    "scrapeContacts": true,
    "minRating": 4
  }'
```

Python (`pip install apify-client`):

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_TOKEN")
run = client.actor("smoldesk/google-maps-scraper").call(run_input={
    "searchTerms": ["plumber"],
    "locations": ["Leeds, UK", "York, UK"],
    "maxPlacesPerSearch": 100,
    "website": "withoutWebsite",
    "requirePhone": True,
})
for place in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(place["name"], place["phone"], place["address"])
```

JavaScript (`npm install apify-client`):

```js
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_TOKEN' });
const run = await client.actor('smoldesk/google-maps-scraper').call({
    searchTerms: ['coworking space'],
    locations: ['Lisbon'],
    maxPlacesPerSearch: 40,
    scrapeContacts: true,
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.map((p) => [p.name, p.emails, p.website]));
```

## Sample output

```json
{
  "placeId": "ChIJb5gBC-u0RIYRHkntaUcdJGs",
  "name": "ATX Family Dental",
  "category": "Dentist",
  "address": "1700 S 1st St, Austin, TX 78704, United States",
  "latitude": 30.2482229,
  "longitude": -97.7559705,
  "rating": 4.9,
  "reviewsCount": 918,
  "phone": "+1 512-717-3147",
  "website": "https://www.atxfamilydental.com/",
  "businessStatus": "OPERATIONAL",
  "claimedByOwner": true,
  "googleMapsUrl": "https://www.google.com/maps/place/?q=place_id:ChIJb5gBC-u0RIYRHkntaUcdJGs",
  "emails": ["info@atxfamilydental.com"],
  "socialProfiles": {"instagram": ["https://www.instagram.com/atxfamilydental"]}
}
```

Export as JSON, CSV or Excel, or pipe it into Google Sheets, Make, Zapier or n8n.

## Pricing

Pay per result. No monthly fee. Prices drop on paid Apify plans.

| Result (per 1,000) | Free | Starter | Scale | Business |
|---|---|---|---|---|
| Place | $2.00 | $1.50 | $1.20 | $1.00 |
| Contact details found (add-on) | +$2.00 | +$1.50 | +$1.20 | +$1.00 |
| Top review | $0.40 | $0.30 | $0.25 | $0.20 |

Filtered-out, duplicate and already-seen places are free. Websites with no email or social profile are free.

## Limits

- Reviews: only the top 5 per place. Google Maps shows the full list only to signed-in users, and this scraper doesn't sign in.
- About 120 places per search (Google's limit). Use smaller locations or `splitArea`.
- Emails come from the business's own website. Some sites list none or block bots. Emails are not verified.
- No popular times, menus, Q&A or owner replies.

## FAQ

**Is there a Google Maps API?**
Google's Places API needs a billing account and charges per request. This reads the public Google Maps pages and charges per place.

**How do I get more than 120 results?**
List neighborhoods or postcodes in `locations`, or set `splitArea: true` (and `splitDepth: 2` for whole cities).

**Can I find businesses without a website?**
Yes: `"website": "withoutWebsite"`, plus `"requirePhone": true` if you want to call them.

**Do I need a Google account or proxies?**
No. It uses no Google account and switches to Apify datacenter proxy on its own if Google slows it down.

## More scrapers

Google Maps Scraper is one of 19 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
