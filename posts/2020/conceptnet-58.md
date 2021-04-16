<!--
.. title: ConceptNet 5.8
.. slug: conceptnet-58
.. date: 2020-05-20 13:20:00 UTC-04:00
.. tags: ConceptNet, Releases
.. category:
.. link:
.. description:
.. type: text
-->

ConceptNet 5.8 has been released!

In this release, we're focused on improving the maintainability of ConceptNet,
with a few small but significant changes to the data. Here's an overview of
what's changed.

## HTTPS support

You can now reach ConceptNet's web site and API over HTTPS. There's nothing
about ConceptNet that particularly requires encryption, but the security of the
Web as a whole would be better if every site could be reached through HTTPS,
and we're happy to go along with that.

One immediate benefit is that an HTTPS web page can safely make requests to
ConceptNet's API.

## Continuous deployment

We now have ConceptNet set up with continuous integration using Jenkins and
deployment using AWS Terraform. This should make new versions and fixes much
easier to deploy, without a long list of things that must be done manually.

This should also allow us, soon, to update the instructions on how to run one's
own copy of the ConceptNet API without so much manual effort.

## Distinguishing Indonesian (`id`) and Malay (`ms`)

The use of the language code `ms` in earlier releases of ConceptNet 5 reflects
our uncertainty about the scope of the `ms` language code. Some sources said it
was a "macrolanguage" code for all Malay languages including Indonesian, so we
implemented it similarly to the macrolanguage `zh` for Chinese languages.
Thus, we formerly used `ms` to include both Indonesian (_bahasa Indonesia_) and
the specific Malay language (_bahasa Melayu_).

This led to confusion of words that have different meanings or connotations in
the two languages, and the appearance that Indonesian was missing from the
language list. In retrospect, it's better and more expected to represent the
Indonesian language with its own language code, `id`.

So in version 5.8, we have separate support for Indonesian (`id`) and Malay
(`ms`). This is the largest data change in ConceptNet 5.8. Fortunately, our
largest sources of data for these languages (Open Multilingual WordNet and
Wiktionary) have similar coverage of both languages.


## Updated French and German Wiktionary

Building ConceptNet involves a step that extracts knowledge from Wiktionary
using our custom MediaWiki parser, [wikiparsec][]. Wiktionary is a
crowd-sourced dictionary that is developed separately in many languages -- that
is, the language that the _definitions_ are in. Each of these languages of
Wiktionary also _defines_ words in hundreds of languages.

[wikiparsec]: https://github.com/LuminosoInsight/wikiparsec

The practices around formatting Wiktionary entries change from time to time, so
a parser for the English Wiktionary of 2019 won't necessarily parse the English
Wiktionary of 2020.

In fact, it doesn't. The above is a problem we've run into. We can't update the
English Wiktionary entries until we account for some entirely new formatting
that arose in the last year. But we can update the other Wiktionaries we parse,
French and German, from the 2016 version to the 2020 version. That's what we've
done in this update, acquiring four years of fixes, new details, and new words.

## Curation of data sources

There's some crowdsourced data that shouldn't appear in ConceptNet, and in 5.8
we're doing more to filter it.

Previously, we've used some heuristics to filter bad answers that came from the
game Verbosity, and a few particularly unhelpful contributors and topic areas
in Open Mind Common Sense.

Recently, we and others have noticed some offensive word associations in
ConceptNet that came from Wiktionary. The issue is that they came from
definitions that are appropriate to find _in a dictionary_, but not elsewhere.
A semantic network isn't a dictionary, and one important difference is that the
edges in ConceptNet appear with no context.

A dictionary can say "X is an offensive term that means Y, and here's where it
came from". It could even have usage notes on why not to say it. That's all
part of a dictionary's job, defining words no matter what they mean, so you can
find out what they mean if you don't know.

In ConceptNet, such an entry ends up as an edge between X and Y, which is the
same as an edge between Y and X. So, unfortunately, looking up an ordinary word
in ConceptNet could produce a list of hateful synonyms, and these word
associations would also be learned by semantic models such as ConceptNet
Numberbatch.

These links aren't worth including in ConceptNet. Fortunately, in many cases,
we can use the structure of Wiktionary to help tell us which edges to _not_
include.

An update to our Wiktionary parser detects definitions that are labeled on
Wiktionary as "offensive" or "slur" or similar labels, and produces metadata
that the build process can use to exclude that definition. With an expansion of
the "blocklist", the file that we use to manually exclude edges, we can also
cover cases that aren't consistently labeled in Wiktionary.

The filtering doesn't just have to be about offensive terms: we also use the
same mechanism to filter definitions that Wiktionary calls archaic and
obsolete, definitions that would not help understand the modern usage of a
word, without affecting other senses of the word.

While I looked through a lot of unfortunate words to check that the filtering
had done the right thing, I know I didn't look at everything, and also I can
only really do this in English. If you see ConceptNet edges that should be
filtered out, feel free to [let me know in an
e-mail](mailto:rspeer@luminoso.com). With continuous integration, I should even
be able to fix it in a timely manner.

This filtering caused no significant change in our semantic benchmarks. As they
say, "nothing of value was lost".


## Cleaning up ExternalURLs

It was during the life of ConceptNet 5.7 that I learned that people _actually
do_ use `ExternalURL` edges to connect ConceptNet to other Linked Open Data
resources. And one user pointed out to me how the presentation of them was...
neglected.

If you're browsing [ConceptNet's web interface][conceptnet], the external links
used to appear as one of the relation types, which led to them being crammed
into a format that didn't really work for them, and also put them in an
arbitrary place on the page depending on how many of them there were. Now, the
external links appear in a differently-formatted section at the bottom.

We also now filter the ExternalURLs to only include terms that exist in
ConceptNet, instead of having isolated terms that only appear in an
ExternalURL.


[conceptnet]: http://conceptnet.io

