<html><body><p>The very informative tutorial by Vlad Niculae on <a href="http://vene.ro/blog/word-movers-distance-in-python.html">Word Mover's Distance in Python</a> includes this step:

</p><blockquote>
  We could train the embeddings ourselves, but for meaningful results we would need tons of documents, and that might take a while. So let’s just use the ones from the word2vec team.
</blockquote>

I couldn't have asked for a better justification for ConceptNet and Luminoso in two sentences.

When presenting new results from <a href="https://blog.conceptnet.io/2016/05/25/conceptnet-numberbatch-a-new-name-for-the-best-word-embeddings-you-can-download/">Conceptnet Numberbatch</a>, which works way better than word2vec alone, one objection is that the embeddings are pre-computed and aren't based on your data. (<a href="http://www.luminoso.com/">Luminoso</a> is a SaaS platform that retrains them to your data, in the cases where you do need that.)

Pre-baked embeddings are useful. People are resigning themselves to use word2vec's pre-baked embeddings because they don't know they can have better ones. I dream of the day when someone writing a new tutorial like this says "So let's just use Conceptnet Numberbatch."</body></html>