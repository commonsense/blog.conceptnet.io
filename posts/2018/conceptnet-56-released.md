<!--
.. title: ConceptNet 5.6 released
.. slug: conceptnet-56-released
.. date: 2018-04-13 17:02:00 UTC-04:00
.. tags: ConceptNet, Releases
.. category: 
.. link: 
.. description: 
.. type: text
-->

ConceptNet 5.6 is out!

We've made a lot of changes behind the scenes that should have fairly small effects on the way you use ConceptNet. Some of the changes are:

- We normalize text properly in more languages. Arabic words no longer insist on matching vowel points that nobody writes in real text. Serbian/Croatian words now have a unified vocabulary written in the Latin alphabet, instead of some words being in the Latin alphabet and some in Cyrillic.

- ConceptNet [knows what emoji are](http://conceptnet.io/c/mul/%F0%9F%98%82) and can define them in a number of languages, thanks to importing Unicode CLDR data. 😺

- We've included data from [CC-CEDICT](https://cc-cedict.org/wiki/), an open Chinese dictionary.

- For fans of self-explaining APIs and what's left of the Semantic Web: Everything returned by the ConceptNet API is now valid [JSON-LD](https://json-ld.org/), and we now test to make sure this is true. You can use [a JSON-LD processor](https://github.com/digitalbazaar/pyld) to convert responses from the ConceptNet API into other formats such as RDF triples.

- We no longer use Docker to deploy ConceptNet. It caused no end of inscrutable problems and it didn't make anything easier. Sorry for getting caught up in the hype. We still provide ways to [configure a machine to serve ConceptNet exactly like we do](https://github.com/commonsense/conceptnet5/wiki/Running-your-own-copy).

More details are on the [changelog](https://github.com/commonsense/conceptnet5/wiki/Changelog) on the ConceptNet wiki.

We also moved our blog -- the one you're reading now -- from WordPress to a static site generated with Nikola. One feature this provides is that we can post Python notebooks [directly on the blog](http://blog.conceptnet.io/posts/2017/how-to-make-a-racist-ai-without-really-trying/), instead of having to use an external service such as Gist. This makes it much easier to post tutorials, and we hope to do this shortly.
