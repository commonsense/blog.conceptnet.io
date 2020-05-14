<!--
.. title: ConceptNet 5.7 released
.. slug: conceptnet-57-released
.. date: 2019-04-30 13:02:00 UTC-04:00
.. tags: ConceptNet, Releases
.. category:
.. link:
.. description:
.. type: text
-->

ConceptNet 5.7 has been released! Here's a tour of some of the things that are new.

## New Japanese knowledge

The work of Naoki Otani, Hirokazu Kiyomaru, Daisuke Kawahara, and Sadao Kurohashi
has expanded and improved ConceptNet's crowdsourced knowledge in Japanese.

This group, from Carnegie Mellon and Kyoto University with support from Yahoo!
Japan, combined translation and crowdsourcing to collect 18,747 new facts in
Japanese, covering common-sense relations that are hard to collect data for,
such as `/r/AtLocation` and `/r/UsedFor`.

You can read the details in their COLING paper, "[Cross-lingual Knowledge
Projection Using Machine Translation and Target-side Knowledge Base
Completion][coling-paper]".

[coling-paper]: https://www.aclweb.org/anthology/C18-1128


## Word senses

<img src="/2019/04/word-senses.png" alt="Different senses of the word 'sense' in ConceptNet.">

Sometimes multiple different meanings happen to be represented by words that are
spelled the same -- that is, different _senses_ of a word.
ConceptNet has vaguely supported word senses for a while, and now we're making
more of an effort to actually support them.

In earlier versions, we only distinguished word senses by their part of speech:
for example, the noun "sense" might be at the ConceptNet URI `/c/en/sense/n`.
Now we can provide more details about word senses when we have them, such as
`/c/en/sense/n/wn/communication`, representing a noun sense of "sense" that's
in the WordNet topic area of "communication".

Of course, a lot of ConceptNet's data comes from natural language text, where
distinguishing word senses is a well-known difficult problem. A lot of the data
is still made of ambiguous words. But when we take input from sources that do
have distinguishable word senses, we include information about the word senses
in the concept URI and in its display on the [browseable ConceptNet
site](http://conceptnet.io).

Specific word senses always appear in a namespace, letting us know who's defining
that word sense, such as WordNet, Wiktionary, or Wikipedia.

Why the different namespaces? Why not have one vocabulary of word senses?
Well, I've never seen a widely-accepted vocabulary of word senses. Every data
source and every word-sense-disambiguation project has its own word senses, and
there's not even agreement on what it takes to say that two word senses are
different.

Some word senses in different namespaces probably ought to be the same sense.
But with different sources distinguishing word senses in different ways, and no
clear way to align them, the best we can do is to distinguish what we can from
each data source, and accept that some of them should be overlapping.


## The new database

ConceptNet 5.7 is powered by PostgreSQL 10, and between the browseable site and
the API, it is queried over 250,000 times per day.

Previously, we had to write carefully-tuned SQL queries so that the server
could quickly respond to all the kinds of intersecting queries that the
ConceptNet API allows, such as [all HasA edges between two nodes in
Japanese][api-example-ja]. Some combinations were previously not supported at
all, such as [all statements collected by Open Mind Common Sense about the
Spanish word _hablar_][api-example-es].

[api-example-ja]: http://api.conceptnet.io/query?node=/c/ja&other=/c/ja&rel=/r/HasA
[api-example-es]: http://api.conceptnet.io/query?source=/s/contributor/omcs&node=/c/es/hablar

Some of the queries were hard to tune. There was some downtime on ConceptNet 5.6
as users started making more difficult queries and the database failed to keep up.
The corners we cut to make the queries efficient enough showed up as strange artifacts,
such as queries where [most of the results would start with "a" or "b"][issue200].

[issue200]: https://github.com/commonsense/conceptnet5/issues/200

With our new database structure, we no longer have to anticipate and tune every
kind of query.  ConceptNet queries are now converted into [JSONB
queries][jsonb], which PostgreSQL 10 knows how to optimize with a new kind of
index. This gives us an efficient way to do every kind of ConceptNet query,
using the work of people who are much better than us at optimizing databases.

[jsonb]: https://www.postgresql.org/docs/10/datatype-json.html


## ConceptNet is part of cutting-edge NLP

Those are the major updates we've made to ConceptNet, so I'd like to wrap up by
discussing why it's important that we continue to maintain and improve
ConceptNet. It continues to play a valuable role in understanding what people
mean by the words they use.

In this area where it seems that machine learning can learn anything from
enough data, it turns out that computers still do gain more common-sense
understanding from ConceptNet. In the [story understanding task at SemEval
2018][semeval2018], the best and fourth-best system both took input from
ConceptNet. We previously wrote about how our ConceptNet-based system was the
second-best at another SemEval task, on [recognizing differences in
attributes][distinguish]. In November 2018, the new [state-of-the-art in the
Story Cloze Test][sct-sota] was set by Jiaao Chen et al. using ConceptNet,
outperforming major systems such as GPT.

If you're using ConceptNet in natural language processing, you should consider
applying to the [Common Sense In NLP workshop][coin] that I'm co-organizing at
EMNLP 2019.

[distinguish]: http://blog.conceptnet.io/posts/2018/distinguishing-attributes-using-conceptnet/
[semeval2018]: https://www.aclweb.org/anthology/S18-1119
[sct-sota]: https://arxiv.org/pdf/1811.00625.pdf
[coin]: https://coinnlp.github.io/
