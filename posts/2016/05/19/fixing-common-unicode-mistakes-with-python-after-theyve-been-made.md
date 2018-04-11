<html><body><h6><em>Originally posted on August 20, 2012.</em></h6>

<strong><em>Update: </em></strong><em>not only can you fix Unicode mistakes with Python, you can <a title="Fixing Unicode mistakes and more: the ftfy package" href="http://blog.lumino.so/2012/08/24/fixing-unicode-mistakes-and-more-the-ftfy-package/">fix Unicode mistakes with our open source </a></em><em><a title="Fixing Unicode mistakes and more: the ftfy package" href="http://blog.lumino.so/2012/08/24/fixing-unicode-mistakes-and-more-the-ftfy-package/">Python package ftfy</a>. It's on PyPI and everything.</em><strong><em>
</em></strong>

You have almost certainly seen text on a computer that looks something like this:

<blockquote>If numbers arenâ€™t beautiful, I donâ€™t know what is. â€“Paul ErdÅ‘s</blockquote>

Somewhere, a computer got hold of a list of numbers that were intended to constitute a quotation and did something distinctly un-beautiful with it. A person reading that can deduce that it was actually supposed to say this:

<blockquote>If numbers aren’t beautiful, I don’t know what is. –Paul Erdős</blockquote>

Here's what's going on. A modern computer has the ability to display text that uses over 100,000 different characters, but unfortunately that text sometimes passes through a doddering old program that believes there are only the 256 that it can fit in a single byte. The program doesn't even bother to check what encoding the text is in; it just uses its own favorite encoding and turns a bunch of characters into strings of completely different characters.

Now, <em>you're</em> not the programmer causing the encoding problems, right? Because you've read something like Joel Spolsky's <a href="http://www.joelonsoftware.com/articles/Unicode.html">The Absolute Minimum Every Developer Absolutely, Positively Must Know About Unicode And Character Sets</a> or the <a href="http://docs.python.org/howto/unicode">Python Unicode HOWTO</a> and you've learned the difference between text and bytestrings and how to get them right.

But the problem is that sometimes you might have to deal with text that comes out of <em>other</em> code. We deal with this a lot at <a href="http://lumino.so">Luminoso</a>, where the text our customers want us to analyze has often passed through several different pieces of software, each with their own quirks, probably with Microsoft Office somewhere in the chain.

So this post isn't about how to do Unicode right. It's about a tool we came up with for damage control after some other program does Unicode wrong. It detects some of the most common encoding mistakes and does what it can to undo them.

<!--more-->

Here's the type of Unicode mistake we're fixing.

<ul>
    <li>Some text, somewhere, was encoded into bytes using UTF-8 (which is quickly becoming the standard encoding for text on the Internet).</li>
    <li>The software that received this text wasn't expecting UTF-8. It instead decodes the bytes in an encoding with only 256 characters. The simplest of these encodings is the one called "ISO-8859-1", or "Latin-1" among friends. In Latin-1, you map the 256 possible bytes to the first 256 Unicode characters. This encoding can arise naturally from software that doesn't even consider that different encodings exist.</li>
    <li>The result is that every non-ASCII character turns into two or three garbage characters.</li>
</ul>

The three most commonly-confused codecs are UTF-8, Latin-1, and Windows-1252. There are lots of other codecs in use in the world, but they are so obviously different from these three that everyone can tell when they've gone wrong. We'll focus on fixing cases where text was encoded as one of these three codecs and decoded as another.

<h1>A first attempt</h1>

When you look at the kind of junk that's produced by this process, the character sequences seem so ugly and meaningless that you could just replace <em>anything</em> that looks like it should have been UTF-8. Just find those sequences, replace them unconditionally with what they would be in UTF-8, and you're done. In fact, that's what my first version did. Skipping a bunch of edge cases and error handling, it looked something like this:

```python


# A table telling us how to interpret the first word of a letter's Unicode
# name. The number indicates how frequently we expect this script to be used
# on computers. Many scripts not included here are assumed to have a frequency
# of "0" -- if you're going to write in Linear B using Unicode, you're
# probably aware enough of encoding issues to get it right.
#
# The lowercase name is a general category -- for example, Han characters and
# Hiragana characters are very frequently adjacent in Japanese, so they all go
# into category 'cjk'. Letters of different categories are assumed not to
# appear next to each other often.
SCRIPT_TABLE = {
    'LATIN': (3, 'latin'),
    'CJK': (2, 'cjk'),
    'ARABIC': (2, 'arabic'),
    'CYRILLIC': (2, 'cyrillic'),
    'GREEK': (2, 'greek'),
    'HEBREW': (2, 'hebrew'),
    'KATAKANA': (2, 'cjk'),
    'HIRAGANA': (2, 'cjk'),
    'HIRAGANA-KATAKANA': (2, 'cjk'),
    'HANGUL': (2, 'cjk'),
    'DEVANAGARI': (2, 'devanagari'),
    'THAI': (2, 'thai'),
    'FULLWIDTH': (2, 'cjk'),
    'MODIFIER': (2, None),
    'HALFWIDTH': (1, 'cjk'),
    'BENGALI': (1, 'bengali'),
    'LAO': (1, 'lao'),
    'KHMER': (1, 'khmer'),
    'TELUGU': (1, 'telugu'),
    'MALAYALAM': (1, 'malayalam'),
    'SINHALA': (1, 'sinhala'),
    'TAMIL': (1, 'tamil'),
    'GEORGIAN': (1, 'georgian'),
    'ARMENIAN': (1, 'armenian'),
    'KANNADA': (1, 'kannada'),  # mostly used for looks of disapproval
    'MASCULINE': (1, 'latin'),
    'FEMININE': (1, 'latin')
}

```</body></html>