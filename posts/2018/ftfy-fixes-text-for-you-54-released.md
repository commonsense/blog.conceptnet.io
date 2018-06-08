<!--
.. title: ftfy (fixes text for you) 5.4 released
.. slug: ftfy-fixes-text-for-you-54-released
.. date: 2018-06-07 14:19:18 UTC-04:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

We've released version 5.4 of _ftfy_, our Python 3 tool that fixes mojibake and other Unicode glitches.

```python
>>> import ftfy

>>> ftfy.fix_text("ongeÃ«venaard")
'ongeëvenaard'

>>> ftfy.fix_text("HÃ”TEL")
'HÔTEL'
```

In this version, we tuned the heuristic to be able to fix more cases where
there are only two characters of mojibake, such as the `Ã«` in
`"ongeÃ«venaard"`, thanks to a bug report about how ftfy was failing to
un-corrupt the letter `ë`.

There are many cases like this that ftfy could fix already, but version 5.3
wasn't convinced it should change anything: "`«` is a kind of quotation mark!
What if the user really meant to put «venaard» in quotes, and there just
happens to be a word ending in Ã right next to it?"

This is a bit of a silly concern when:

- quotation marks aren't usually sandwiched directly between letters, with
  no spaces around them

- words don't usually end in `Ã`, not even in Portuguese

- The text would really look a lot better with `ë` in it instead of `Ã«`

We tuned the heuristic so that it recognizes more of these two-character
sequences as clear cases of mojibake, and doesn't worry about quotation marks
that are between letters.

Why does ftfy have to be careful in cases like this? It may seem that we could
just fix every two-character sequence that looks like Windows-1252 was mixed up
with UTF-8, the most common form of mojibake. But one design goal is that we
really don't want it to _introduce_ errors. Here's a real-world example that's
in ftfy's tests:

```python
>>> text = "PARCE QUE SUR LEURS PLAQUES IL Y MARQUÉ…"

>>> # It's possible to decode this text as if it's mojibake.
>>> text.encode('windows-1252').decode('utf-8')
'PARCE QUE SUR LEURS PLAQUES IL Y MARQUɅ'

>>> # But we don't, because the text is fine as it is.
>>> ftfy.fix_text(text)
'PARCE QUE SUR LEURS PLAQUES IL Y MARQUÉ…'
```

People are often surprised that ftfy is a hand-tuned heuristic, and not, for
example, the output of a machine-learning algorithm. Machine learning is great,
but it has its limits. One advantage of being hand-tuned is that we can keep
aiming for a false positive rate that's so low that an ML training loop
wouldn't even be able to measure it. Another advantage, shown with this update,
is that we can make sure to do the right thing in these minimal cases.

Machine-learned tools such as the language detector `cld2` will warn you that
they're "not designed to do well on short text". Short text is often interesting
and important, so ftfy is designed to do well on it.

Also with this release, we can finally have a [nice-looking project
page](https://pypi.org/project/ftfy/) on the new Python Package Index.
