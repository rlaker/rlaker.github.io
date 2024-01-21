---
title: "What's my most controversial movie opinion?"
layout: single
excerpt: "Exploring my Letterboxd data from the last year"
---


<iframe src="/files/letterboxd/diff_ratings.html" width="120%" height="500px" style="border:none;"></iframe>
*Hover over the interactive plots, made with [Plotly](https://plotly.com/python/)*


It turns out the film I value more than most on [Letterboxd](https://letterboxd.com/) is `22 Jump Street` and my most disappointing was `The Terminator`. Now, comedies are subjective, so I feel no need to defend one of my favourites, but I might get some flack for my Terminator review. In my defence, this film is so important for sci-fi that I knew the whole plot and infamous moments before watching. Maybe its my own fault, but I wasn't expecting it to be quite so low budget, particularly the mirror scene...


## Superior Streaming Service?

<iframe src="/files/letterboxd/watch_type.html" width="120%" height="500px" style="border:none;"></iframe>

Interestingly, I watched more than twice as many films on streaming services than in the cinema, but which is superior?

From the below figure, the box plots show that I found films on Netflix the highest quality overall (excluding iplayer which only had 3 films). While the cinema had the largest spread in ratings, it did provide the most 5 star films: Barbie, Oppenheimer, Mission: Impossible Dead Reckoning and 2001: A Space Odyssey

<iframe src="/files/letterboxd/which_service.html" width="120%" height="500px" style="border:none;"></iframe>

Can you guess when I finished my PhD thesis?

<iframe src="/files/letterboxd/type_over_time.html" width="120%" height="500px" style="border:none;"></iframe>

## Getting average ratings from Letterboxd

While Letterboxd provides some neatly packaged `.csv` files for users to explore their own data, the API for gathering information about each film is private. Therefore, to compare my ratings with those of the general public, I needed to first find the average rating of each film. 

Originally I turned the `Name` column into a hyphen separated URL, such as <https://letterboxd.com/film/no-time-to-die/>. However, this referenced a little known film about a hearse driver, rather than the James Bond one I had seen, with the actual URL of <https://letterboxd.com/film/no-time-to-die-2021/>.

Within the `.csv` files, Letterboxd provide the url of each of personal film review, such as <https://boxd.it/41lX6F>. Thankfully, this redirected to the full address like <https://letterboxd.com/rlaker/film/no-time-to-die-2021/>, which just needed my username removing with the following function:

```python
import requests
import pandas as pd

watched = pd.read_csv("watched.csv")

def get_final_url(redirect_url):
    try:
        response = requests.get(redirect_url)
        if response.history:
            # Extract final URL after redirection
            final_url = response.url
            # Remove '/rlaker' part from the URL
            final_url = final_url.replace('/rlaker', '')
            return final_url
        else:
            return "No redirection occurred"
    except requests.RequestException as e:
        return str(e)

watched['Letterboxd_URL'] = watched['Letterboxd URI'].apply(get_final_url)
```

After originally trying to find the right `<span>` with [BeautifulSoup](https://beautiful-soup-4.readthedocs.io/en/latest/), I realised the data was actually stored in a dictionary, which was then served by Javascript later (as explained in this Reddit [answer](https://www.reddit.com/r/learnpython/comments/howbg1/scraping_data_from_letterboxd_using_beautifulsoup/)). 

It turned out regular expressions were enough for this task:

```python
import requests
import re  # Regular expression module

def get_average_rating(url):
    try:
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.3'
        }
        response = requests.get(url, headers=headers)
        if response.status_code == 200:
            # Use regular expression to find "ratingValue"
            match = re.search(r'"ratingValue":(\d+\.\d+)', response.text)
            if match:
                return float(match.group(1)) # The first group captures the rating value
            else:
                return None
        else:
            return None
    except Exception as e:
        return str(e)

# Applying the function to each URL
watched['Average_Rating'] = watched['Letterboxd_URL'].apply(get_average_rating)
```
