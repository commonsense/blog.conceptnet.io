<html><body>

<a href="https://gist.github.com/rspeer/0a5dc7e6983d2ce752b5"><img class="wp-image-823 size-full" src="https://luminosoinsight.files.wordpress.com/2015/10/wordfreq-1-2-examples.png" alt="Wordfreq 1.2 example code" width="407" height="203"></a> Examples in Chinese and British English. Click through for copyable code.

<a href="https://conceptnetblog.wordpress.com/2016/05/19/wordfreq-open-source-and-open-data-about-word-frequencies/" target="_blank" rel="noopener noreferrer">In a previous post</a>, we introduced <em><a href="https://github.com/LuminosoInsight/wordfreq">wordfreq</a></em>, our open-source Python library that lets you ask "how common is this word?"

Wordfreq is an important low-level tool for Luminoso. It's one of the things we use to figure out which words are important in a set of text data. When we get the word frequencies figured out in a language, that's a big step toward being able to handle that language from end to end in the Luminoso pipeline. We recently started supporting Arabic in our product, and improved Chinese enough to take the "BETA" tag off of it, and having the right word frequencies for those languages was a big part of it.

I've continued to work on wordfreq, putting together more data from more languages. We now have 17 languages that meet the threshold of having three independent sources of word frequencies, which we consider important for those word frequencies to be representative.

Here's what's new in wordfreq 1.2:

<ul>
    <li>The <strong>English</strong> word list has gotten a bit more robust and a bit more British by including SUBTLEX, adding word frequencies from American TV shows as well as the BBC.</li>
    <li>It can fearlessly handle <strong>Chinese</strong> now. It uses a lovely pure-Python Chinese tokenizer, <a href="https://github.com/fxsjy/jieba">Jieba</a>, to handle multiple-word phrases, and Jieba's built-in wordlist provides a third independent source of word frequencies. Wordfreq can even smooth over the differences between Traditional and Simplified Chinese.</li>
    <li><strong>Greek</strong> has also been promoted to a fully-supported language. With new data from Twitter and OpenSubtitles, it now has four independent sources.</li>
    <li>In some applications, you want to tokenize a complete piece of text, including punctuation as separate tokens. Punctuation tokens don't get their own word frequencies, but you can ask the tokenizer to give you the punctuation tokens anyway.</li>
    <li>We added support for <strong>Polish</strong>, <strong>Swedish</strong>, and <strong>Turkish</strong>. All those languages have a reasonable amount of data that we could obtain from OpenSubtitles, Twitter, and Wikipedia by doing what we were doing already.</li>
</ul>

When adding Turkish, we made sure to convert the case of dotted and dotless İ's correctly. We know that putting the dots in the wrong places can lead to miscommunication and even <a href="http://gizmodo.com/382026/a-cellphones-missing-dot-kills-two-people-puts-three-more-in-jail">fatal stabbings</a>.

The language in wordfreq that's still only partially supported is Korean. We still only have two sources of data for it, so you'll see the disproportionate influence of Twitter on its frequencies. If you know where to find a lot of freely-usable Korean subtitles, for example, we would love to know.

Let's revisit the top 10 words in the languages wordfreq supports. And now that we've talked about <a href="http://blog.luminoso.com/2015/09/21/can-we-do-arabic/">getting right-to-left right</a>, let's add a bit of code that makes Arabic show up with right-to-left words in left-to-right order, instead of middle-to-elsewhere order like it came out before.

<img class="aligncenter wp-image-832 size-full" src="https://luminosoinsight.files.wordpress.com/2015/10/wordfreq-1-22.png" alt="Code showing the top ten words in each language wordfreq 1.2 supports." width="764" height="684">

Wordfreq 1.2 is available on <a href="https://github.com/LuminosoInsight/wordfreq">GitHub</a> and <a href="https://pypi.python.org/pypi/wordfreq">PyPI</a>.</body></html>
