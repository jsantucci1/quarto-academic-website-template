# Is negative tone in Trump's 2019 posts concentrated in conflict themes?

POLS 4130 · Text Analysis Project · Fall 2026
Juliana Santucci

Poster: https://jsantucci1.github.io/quarto-academic-website-template/

## Research question

Is negative tone in Trump's 2019 posts concentrated in conflict-driven themes (impeachment, the Russia probe and the media) rather than in policy and diplomacy themes, and does it shift over the year?

Expectation: negativity is concentrated in the conflict theme, while endorsements and diplomacy stay positive.

## Data

- **Posts:** 4,469 posts from one account, January–December 2019. One row is one post. Source: Mark Huang, *Complete Trump Tweets Archive* (https://github.com/MarkHershey/CompleteTrumpTweetsArchive, commit `0de31782d856523e0fce5022a993781caf317d9f`). The course preparation selected 7,249 records dated 2019, excluded 2,498 retweets ("RT @") and 281 records with no alphabetic text after URL removal, and removed one duplicate import. URLs were removed and whitespace collapsed.
- **Dictionary:** Hu and Liu (2004), *Mining and summarizing customer reviews*, distributed through tidytext. 6,556 words scored +1 (positive) or −1 (negative).
- These are one account's messages, not public opinion. Source notices are in `data/SOURCE_NOTICES.txt`.

## Repository contents

```
Week9_R.Rproj             RStudio project (open this first)
Week9_Project_Final.R     Full analysis script (run top to bottom)
research_poster.qmd       Quarto poster
README.md                 This file
data/
  trump_tweets_2019.csv   Posts
  bing_liu_dictionary.csv Sentiment dictionary
  SOURCE_NOTICES.txt      Source and adaptation notes
outputs/                  Figures used on the poster (ten PNGs)
```

## How to reproduce

1. Install R (the analysis was run in R 4.6.1) and RStudio.
2. Install the packages once:
   ```r
   install.packages(c("dplyr", "stringr", "tidytext", "ggplot2",
                      "textstem", "topicmodels", "reshape2",
                      "wordcloud", "slider"))
   ```
3. Open `Week9_R.Rproj` so the working directory is the project folder.
4. Open `Week9_Project_Final.R` and run it one block at a time, in order, from W9-1 to W9-8. Later blocks use objects created by earlier ones.
5. The script saves the figures to `outputs/`. The word cloud is saved by hand: run the `wordcloud()` line, then Plots > Export > Save as Image as `outputs/wordcloud.png`.
6. Open `research_poster.qmd` and click Render to build `research_poster.html`. The poster reads the PNGs from `outputs/`.

The topic model uses a fixed method and seed (Gibbs sampling, seed 4130), so the topics reproduce. Block W9-5B refits other values of k for comparison; it does not change the main k = 3 model.

## Methods

**Sentiment.** Posts are split into words (original forms, stopwords kept). Each word matching the dictionary adds +1 or −1, and the sum is the post's net score. Group averages weight each post equally. Only about 9–13% of words per post match the dictionary on average. A negation rule flips a dictionary word when "not," "no" or "never" appears in the previous two words. Target tone sums negation-adjusted scores within four words of "democrats," "media" or "china."

**Topics.** Words are lemmatized and SMART stopwords are removed. LDA with k = 3 is fit to the document-term matrix (4,441 posts; 28 posts had no retained words and were excluded from topic modeling). I compared k = 2, 3 and 4 and kept k = 3: k = 2 merged endorsement/border posts with diplomacy/economy posts, and k = 4 split the endorsement/border group further, but I could support only three themes with original posts. Topic labels come from the top words and the highest-weight posts for each topic. Each post is assigned to its strongest topic for the shares and sentiment-by-topic results.

## Main findings

- Impeachment/Russia/media posts average a net score of −0.84 and are negative in all 12 months (−0.36 in November to −1.39 in March). Endorsements/border/jobs (+0.80) and diplomacy/trade/economy (+0.99) are positive in all 12 months.
- The impeachment/media theme grows from roughly 25–31% of posts in early and mid-year months to about 44% in September–December, while becoming less negative.
- Handling negation raised the corpus mean from 0.252 to 0.299 and changed the score of 312 posts.
- Tone within four words of a target: media −0.84, Democrats −0.32, China +0.11.

## Limitations

- The dictionary misses sarcasm, quotations and who a word is about. Only about 10% of words match.
- The negation rule looks back two words and can misfire. Target windows measure tone near a mention, not about it, and match only exact words.
- Topic 3 is loose, k is a judgment call, and the sentiment gap between topics is partly circular because negative dictionary words ("fake," "bad") are keywords of the negative topic.
- Evidence posts are top-weighted or most extreme examples, not typical ones. Results describe how a dictionary scores one account's posts, not the president's beliefs, public reaction or causes.

## Notes on AI assistance
- I used Claude to help correct errors in the code, but I ran all code myself and checked the output against the original posts. 
