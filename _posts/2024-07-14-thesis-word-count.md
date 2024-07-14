---
title: "Using git to track how I wrote my PhD thesis"
excerpt: "One of the many benefits of using git to help write your thesis is that you can go back in time through the project and see how you progressed"
layout: single
tags: [visualization]
---

![](/images/word_count.png)


One of the many benefits of using git to help write your thesis is that you can go back in time through the project and see how you progressed. Overall, the writing took 6 months with a real flurry of words in the final stretch as I quickly typed up the background information in Chapter 2.

While the final thesis was written in Latex, and synced to Overleaf with git, I found it easier to write my first drafts on pen and paper, so that I could easily chop and change the order when translating into its digital form. As a result, the word count would jump by thousands of words.

I like how you can clearly see my strategy of typing up a chapter as a whole, and then moving on to the next (with a gap for Christmas obviously).

As I am typing this up, I just thought about how cool it would be to see a timelapse of the final PDF over time in order to show how the order and content of figures evolved... but that might have to be saved for a later day.

For now, I will just share the code to recreate this initial figure. Originally, I thought I would have to write some crazy bash script to move backwards in time through the git repository. It turns out there is just a Python package that does this remarkably easily, as the code below shows:

```python
import git
import os
import matplotlib.pyplot as plt
import pandas as pd
from datetime import datetime
from tqdm import tqdm
plt.style.use('./style.mplstyle')

# Function to count words in a file
def word_count(filepath):
    with open(filepath, 'r', encoding='utf-8') as file:
        text = file.read()
    words = text.split()

    # write a version that ignores % in .tex files

    # could write a better version of this if you really want
    return len(words)

repo_path = 'Thesis/'  # Update this path to your local repository path
repo = git.Repo(repo_path)

# Initialize lists to store data
dates = []
file_word_counts = {}

# Get all commits
commits = list(repo.iter_commits())

# Iterate through commits
for commit in tqdm(commits):
    repo.git.checkout(commit)
    commit_date = datetime.fromtimestamp(commit.committed_date)
    dates.append(commit_date)

    for root, dirs, files in os.walk(repo_path):
        for file in files:
            if file.endswith('.tex'):  # Assuming your thesis is written in LaTeX
                filepath = os.path.join(root, file)
                words = word_count(filepath)

                if filepath not in file_word_counts:
                    file_word_counts[filepath] = []
                file_word_counts[filepath].append((commit_date.strftime('%Y-%m-%d'), words))
        
        commit_date.strftime('%Y-%m-%d')
        

# Reset to the original head
repo.git.checkout('master')

#put data into a pandas dataframe
final_df = pd.DataFrame()

for key, item in file_word_counts.items():
    df1 = pd.DataFrame(item, columns=['date', 'word_count']).drop_duplicates()
    df1['date'] = pd.to_datetime(df1['date'])
    df1 = df1.groupby('date').max().reset_index()
    
    df1['filename'] = key

    final_df = pd.concat((final_df, df1))


# Plot data
fig, axs = plt.subplots(1,1, figsize = (10,8))

files_to_plot = [
    'Thesis.tmp/1introduction.tex',
    'Thesis.tmp/chapter2.tex',
    'Thesis.tmp/chapter3.tex',
    'Thesis.tmp/chapter4.tex',
    'Thesis.tmp/chapter5.tex',
    'Thesis.tmp/conclusion.tex'
]
import numpy as np
total = np.zeros(final_df.shape[0])
for filepath in files_to_plot:
    df_file = final_df[final_df['filename'] == filepath]

    axs.plot(df_file['date'], df_file['word_count'], label=os.path.basename(filepath))

axs.set_xlim(datetime(2022,9,1), datetime(2023,4,1))
axs.set_ylabel('Word Count')
axs.legend(loc = 'upper left', facecolor='white')

import matplotlib.ticker as ticker

# Just put a , between 000
axs.yaxis.set_major_formatter(ticker.StrMethodFormatter('{x:,.0f}'))

from matplotlib import dates as mdates
axs.xaxis.set_major_formatter(mdates.DateFormatter('%Y-%b-%d'))

for label in axs.get_xticklabels(which='major'):
	label.set(rotation=45, horizontalalignment='right')

fig.tight_layout()
```