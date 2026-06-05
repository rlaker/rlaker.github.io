---
title: "Showing my Unsplash gallery on my blog"
layout: single
excerpt: "How I embedded my Unsplash photos on my Jekyll site without exposing an API key"
tags: [code,til]
---

I wanted to show my [Unsplash photos](https://unsplash.com/@rlaker) directly on my [photography page](/photos/) instead of just showing a text link. Since Unsplash doesn't have an embed widget, I needed to use their API.

## The naive approach (and why I didn't use it)

The simplest option is to call the Unsplash API from JavaScript on the client side:

```javascript
fetch('https://api.unsplash.com/users/rlaker/photos', {
  headers: { 'Authorization': 'Client-ID YOUR_ACCESS_KEY' }
})
```

This works, but it has two problems:

1. **Your API key is visible in the page source.** Unsplash's demo keys are read-only and rate-limited, so it's not catastrophic, but it's not great either.
2. **Rate limiting.** The demo tier allows 50 requests per hour. Each page view is one request that collects the metadata of all my images on unsplash (the photos themselves are served from Unsplash's CDN and don't count as requests).

## The build-time approach

Instead, with the help of Claude, I fetch the photos at build time using a GitHub Actions workflow. The workflow runs weekly (and can be triggered manually), calls the Unsplash API, and saves the results to `_data/unsplash.json`. Jekyll reads this file as `site.data.unsplash` during the build, so the photos are baked into the HTML meaning no API calls when someone views the page.

### The GitHub Action

```yaml
name: Fetch Unsplash photos

on:
  schedule:
    - cron: "0 6 * * 1"  # every Monday at 6am
  workflow_dispatch:       # manual trigger

jobs:
  fetch:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Fetch photos from Unsplash
        env:
          UNSPLASH_ACCESS_KEY: {% raw %}${{ secrets.UNSPLASH_ACCESS_KEY }}{% endraw %}
        run: |
          # fetch all photos, paginating through results
          # save to _data/unsplash.json

      - name: Commit if changed
        run: |
          git add _data/unsplash.json
          git diff --cached --quiet && exit 0
          git commit -m "update Unsplash photo data"
          git push
```

The API key lives in a GitHub secret and never touches the repo or the client.

### The template

The photos page loops over the data file using Liquid:

```liquid
{% raw %}{% for photo in site.data.unsplash %}
  <a href="{{ photo.url }}">
    <img src="{{ photo.src }}" alt="{{ photo.alt }}" loading="lazy">
  </a>
{% endfor %}{% endraw %}
```

I show the first 12 photos and hide the rest behind a "Load more" button that just removes `display:none` — no JavaScript fetch needed.

### The data file

Each entry in `unsplash.json` looks like:

```json
{
  "url": "https://unsplash.com/photos/...",
  "src": "https://images.unsplash.com/...?w=400",
  "alt": "photo description",
  "description": "longer description"
}
```

The `src` field uses Unsplash's `small` size (400px wide), which is plenty for a thumbnail grid and keeps the page fast.

## Setup steps

1. Create an app on [unsplash.com/developers](https://unsplash.com/developers) and grab the Access Key
2. Add it as a GitHub secret called `UNSPLASH_ACCESS_KEY` (repo Settings > Secrets and variables > Actions)
3. Run the workflow manually once to generate the initial `_data/unsplash.json`
4. After that, the workflow refreshes it weekly

## The result

My [photos page](/photos/) now shows my latest Unsplash uploads at the top, automatically refreshed every week, with no API key exposed and no client-side rate limiting to worry about.
