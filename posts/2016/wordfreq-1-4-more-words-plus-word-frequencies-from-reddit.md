The <a href="https://github.com/LuminosoInsight/wordfreq">wordfreq</a> module is an easy Python interface for looking up the frequencies of words. It was originally designed for use cases where it was most important to find common words, so it would list all the words that occur at least once per million words: that's about 30,000 words in English. An advantage of ending the list there is that it loads really fast and takes up a small amount of RAM.

But there's more to know about word frequencies. There's a difference between words that are used a bit less than once in a million words, like "almanac", "crusty", and "giraffes", versus words that are used just a few times per billion, such as "centerback", "polychora", and "scanlations". As I've started using wordfreq in some aspects of the build process of ConceptNet, I've wanted to be able to rank words by frequency even if they're less common than "giraffes", and I'm sure other people do too.

So one big change in wordfreq 1.4 is that there is now a 'large' wordlist available in the languages that have enough data to support it: English, German, Spanish, French, and Portuguese. These lists contain all words used at least once per 100 million words. The default wordlist is still the smaller, faster one, so you have to ask for the 'large' wordlist explicitly -- see the <a href="https://github.com/LuminosoInsight/wordfreq">documentation</a>.

</p><h2>Including word frequencies from Reddit</h2>

The best way to get representative word frequencies is to include a lot of text from a lot of different sources. Now there's another source available: the <a href="https://www.reddit.com/r/datasets/comments/3bxlg7/i_have_every_publicly_available_reddit_comment">Reddit comment corpus</a>.

Reddit is an English-centric site and 99.2% of its comments are in English. We still need to account for the exceptions, such as <a href="http://reddit.com/r/es">/r/es</a>, <a href="http://reddit.com/r/todayilearned_jp">/r/todayilearned_jp</a>, <a href="http://reddit.com/r/sweden">/r/sweden</a>, and of course, the thread named “<a href="https://www.reddit.com/comments/cq1q2/help_reddit_turned_spanish_and_i_cannot_undo_it/">HELP reddit turned spanish and i cannot undo it!</a>”.

I used <a href="https://github.com/aboSamoor/pycld2">pycld2</a> to detect the language of Reddit comments. In this version, I decided to only use the comments that could be detected as English, because I couldn't be sure that the data I was getting from other languages was representative enough. For example, unfortunately, most comments in Italian on Reddit are spam, and most comments in Japanese are English speakers trying to learn Japanese. The data that looks the most promising is Spanish, and I might decide to include that in a later version.

So now some Reddit-centric words have claimed a place in the English word list, alongside words from Google Books, Wikipedia, Twitter, television subtitles, and the Leeds Internet Corpus:

```python
>>> wordfreq.zipf_frequency('people', 'en', 'large')
6.23

>>> wordfreq.zipf_frequency('cats', 'en', 'large')
4.42

>>> wordfreq.zipf_frequency('giraffes', 'en', 'large')
3.0

>>> wordfreq.zipf_frequency('narwhals', 'en', 'large')
2.1

>>> wordfreq.zipf_frequency('heffalumps', 'en', 'large')
1.78

>>> wordfreq.zipf_frequency('borogoves', 'en', 'large')
1.16
```

wordfreq is part of a stack of natural language tools developed at <a href="http://luminoso.com">Luminoso</a> and used in <a href="http://conceptnet.io">ConceptNet</a>. Its data is available under the <a href="https://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0</a> license.
