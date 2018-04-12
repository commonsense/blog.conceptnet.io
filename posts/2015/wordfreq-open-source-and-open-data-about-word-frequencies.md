<small><em>Unfortunately, the images in this post have been lost to history. We blame WordPress, which we don't use anymore. We recommend reading [a more recent post](/posts/2016/wordfreq-1-5-more-data-more-languages-more-accuracy/) anyway.</em></small>

Often, in NLP, you need to answer the simple question: "is this a common word?" It turns out that this leaves the computer to answer a more vexing question: "What's a word?"

Let's talk briefly about why word frequencies are important. In many cases, you want to assign more significance to uncommon words. For example, a product review might contain the word "use" and the word "defective", and the word "defective" carries way more information. If you're wondering what the deal is with John Kasich, a headline that mentions "Kasich" will be much more likely to be what you're looking for than one that merely mentions "John".

For purposes like these, it would be nice if we could just import a Python package that could tell us whether one word was more common than another, in general, based on a wide variety of text. We looked for a while and couldn't find it. So we built it.

<a href="https://github.com/LuminosoInsight/wordfreq">wordfreq</a> provides estimates of the frequencies of words in many languages, loading its data from efficiently-compressed data structures so it can give you word frequencies down to 1 occurrence per million without having to access an external database. It aims to avoid being limited to a particular domain or style of text, getting its data from a variety of sources: Google Books, Wikipedia, OpenSubtitles, Twitter, and the Leeds Internet Corpus.

<a href="https://luminosoinsight.files.wordpress.com/2015/08/most-common-words2.png"><img class="size-full wp-image-767" src="https://luminosoinsight.files.wordpress.com/2015/08/most-common-words2.png" alt="The 10 most common words that wordfreq knows in 15 languages." width="808" height="376"></a> The 10 most common words that <em>wordfreq</em> knows in 15 languages. Yes, it can handle multi-character words in Chinese and Japanese; those just aren't in the top 10. A puzzle for Unicode geeks: guess where the start of the Arabic list is.

<h2>Partial solutions: stopwords and inverse document frequency</h2>

Those who are familiar with the basics of information retrieval probably have a couple of simple suggestions in mind for dealing with word frequencies.

One is to come up with a list of <em>stopwords</em>, words such as "the" and "of" that are too common to use for anything. Discarding stopwords can be a useful optimization, but that's far too blunt of an operation to solve the word frequency problem in general. There's no place to draw the bright line between stopwords and non-stopwords, and in the "John Kasich" example, it's not the case that "John" should be a stopword.

Another partial solution would be to collect all the documents you're interested in, and re-scale all the words according to their <em>inverse document frequency</em> or IDF. This is a quantity that decreases as the proportion of documents a word appears in increases, reaching 0 for a word that appears in every document.

One problem with IDF is that it can't distinguish a word that appears in a lot of documents because it's unimportant, from a word that appears in a lot of documents because it's <em>very</em> important to your domain. Another, more practical problem with IDF is that you can't calculate it until you've seen all your documents, and it fluctuates a lot as you add documents. This is particularly an issue if your documents arrive in an endless stream.

We need good domain-general word frequencies, not just domain-specific word frequencies, because without the general ones, we can't determine which domain-specific word frequencies are interesting.

<h2>Avoiding biases</h2>

The counts of one resource alone tend to tell you more about that resource than about the language. If you ask Wikipedia alone, you'll find that "census", "1945", and "stub" are very common words. If you ask Google Books, you'll find that "propranolol" is supposed to be 10 times more common than "lol" overall (and also that there's something funny going on, so to speak, in the early 1800s).

<a href="https://luminosoinsight.files.wordpress.com/2015/08/propranolol.png"><img class="alignnone size-large wp-image-762" src="https://luminosoinsight.files.wordpress.com/2015/08/propranolol.png?w=660" alt="" width="660" height="241"></a>

If you collect data from Twitter, you'll of course find out how common "lol" is. You also might find that the ram emoji "🐏" is supposed to be extremely common, because that guy from One Direction once tweeted "We are derby super 🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏🐏", and apparently every fan of One Direction who knows what Derby Super Rams are retweeted it.

Yes, wordfreq considers emoji to be words. Its Twitter frequencies would hardly be complete without them.

We can't entirely avoid the biases that come from where we get our data. But if we collect data from enough different sources (not just larger sources), we can at least smooth out the biases by averaging them between the different sources.

<h2>What's a word?</h2>

You have to agree with your wordlist on the matter of what constitutes a "word", or else you'll get weird results that aren't supported by the actual data.

Do you split words at all spaces and punctuation? Which of the thousands of symbols in Unicode are punctuation? Is an apostrophe punctuation? Is it punctuation when it puts a word in single quotes? Is it punctuation in "can't", or in "l'esprit"? How many words is "U.S." or "google.com"? How many words is "お早うございます" ("good morning"), taking into account that Japanese is written without spaces? The symbol "-" probably doesn't count as a word, but does "+"? How about "☮" or "♥"?

The process of splitting text into words is called "tokenization", and everyone's got their own different way to do it, which is a bit of a problem for a word frequency list.

We tried a few ways to make a sufficiently simple tokenization function that we could use everywhere, across many languages. We ended up with our own ad-hoc rule including large sets of Unicode characters and a special case for apostrophes, and this is in fact what we used when we originally released wordfreq 1.0, which came packaged with regular expressions that look like attempts to depict the Flying Spaghetti Monster in text.

<a href="https://luminosoinsight.files.wordpress.com/2015/08/ugly-regex.png"><img class="aligncenter wp-image-769 size-full" src="https://luminosoinsight.files.wordpress.com/2015/08/ugly-regex.png" alt="A particularly noodly regex." width="148" height="136"></a>

But shortly after that, I realized that <a href="http://unicode.org/reports/tr29/">the Unicode Consortium had already done something similar</a>, and they'd probably thought about it for more than a few days.

<a href="https://luminosoinsight.files.wordpress.com/2015/08/boundary-fig-2.gif"><img class="size-full wp-image-768" src="https://luminosoinsight.files.wordpress.com/2015/08/boundary-fig-2.gif" alt="Word splitting in Unicode" width="361" height="65"></a> Word splitting in Unicode. Not pictured: how to decide which of these segments count as "words".

This standard for tokenization looked like almost exactly what we wanted, and the last thing holding me back was that implementing it efficiently in Python looked like it was going to be a huge pain. Then I found that the <a href="https://pypi.python.org/pypi/regex">regex package</a> (not the <em>re</em> package built into Python) contains an efficient implementation of this standard. Defining how to split text into words became a very simple regular expression... except in Chinese and Japanese, because a regular expression has no chance in a language where the separation between words is not written in any way.

So this is how wordfreq 1.1 identifies the words to count and the words to look up. Of course, there is going to be data that has been tokenized in a different way. When wordfreq gets something that looks like it should be multiple words, it will look them up separately and estimate their combined frequency, instead of just returning 0.

<strong>Language support</strong>

wordfreq supports 15 commonly-used languages, but of course some languages are better supported than others. English is quite polished, for example, while Chinese so far is just there to be better than nothing.

The reliability of each language corresponds pretty well with the number of different data sources we put together to make the wordlist. Some sources are hard to get in certain languages. Perhaps unsurprisingly, for example, not much of Twitter is in Chinese. Perhaps more surprisingly, not much of it is in German either.

The word lists that we've built for wordfreq represent the languages where we have at least two sources. I would consider the ones with two sources a bit dubious, while all the languages that have three or more sources seem to have a reasonable ranking of words.

<ul>
    <li><strong>5 sources</strong>: English</li>
    <li><strong>4 sources</strong>: Arabic, French, German, Italian, Portuguese, Russian, Spanish</li>
    <li><strong>3 sources</strong>: Dutch, Indonesian, Japanese, Malay</li>
    <li><strong>2 sources</strong>: Chinese, Greek, Korean</li>
</ul>

<h2>Compact wordlists</h2>

When we were still figuring this all out, we made several 0.x versions of wordfreq that required an external SQLite database with all the word frequencies, because there are millions of possible words and we had to store a different floating-point frequency for each one. That's a lot of data, and it would have been infeasible to include it all inside the Python package. (GitHub and PyPI don't like huge files.) We ended up with a situation where installing wordfreq would either need to download a huge database file, or build that file from its source data, both of which would consume a lot of time and computing resources when you're just trying to install a simple package.

As we tried different ways of shipping this data around to all the places that needed it, we finally tried another tactic: What if we just distributed less data?

Two assumptions let us greatly shrink our word lists:

<ul>
    <li>We don't care about the frequencies of words that occur less than once per million words. We can just assume all those words are equally informative.</li>
    <li>We don't care about, say, 2% differences in word frequency.</li>
</ul>

Now instead of storing a separate frequency for each word, we group the words into 600 possible tiers of frequency. You could call these tiers "centibels", a logarithmic unit similar to decibels, because there are 100 of them for each factor of 10 in the word frequency. Each of them represents a band of word frequencies that spans about a 2.3% difference. The data we store can then be simplified to "Here are all the words in tier #330... now here are all the words in tier #331..." and converted to frequencies when you ask for them.

<a href="https://luminosoinsight.files.wordpress.com/2015/08/wordfreq-tiers.png"><img class="size-full wp-image-784" src="https://luminosoinsight.files.wordpress.com/2015/08/wordfreq-tiers.png" alt="Some tiers of word frequencies in English." width="813" height="182"></a> Some tiers of word frequencies in English.

This let us cut down the word lists to an entirely reasonable size, so that we can put them in the repository, and just keep them in memory while you're using them. The English word list, for example, is 245 KB, or 135 KB compressed.

But it's important to note the trade-off here, that wordfreq only represents sufficiently common words. It's not suited for comparing rare words to each other. A word rarer than "amulet", "bunches", "deactivate", "groupie", "pinball", or "slipper", all of which have a frequency of about 1 per million, will not be represented in wordfreq.

<h2>Getting the package</h2>

wordfreq is available <a href="https://github.com/LuminosoInsight/wordfreq">on GitHub</a>, or it can be installed from the Python Package Index with the command <i>pip install wordfreq</i>. Documentation can be found in <a href="https://github.com/LuminosoInsight/wordfreq">its README on GitHub</a>.

<a href="https://luminosoinsight.files.wordpress.com/2015/08/cafe-frequency.png"><img class="size-full wp-image-771" src="https://luminosoinsight.files.wordpress.com/2015/08/cafe-frequency.png" alt="wordfreq usage example" width="364" height="219"></a> Comparing the frequency per million words of two spellings of "café", in English and French.

