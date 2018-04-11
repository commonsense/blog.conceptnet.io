<html><body><p>If you have used the ConceptNet Numberbatch 17.04 word vectors, it turns out that you got very different results if you downloaded the English-only vectors versus if you used the multilingual, language-tagged vectors.

I decided to make this downloadable file of English-only vectors as a convenience, because it would be the format that looked most like a drop-in replacement for word2vec's data. But the English-only format is not a format that we use anywhere. We test our vectors, but we don't test reimporting them from all the files we exported, so that caused a bug in the export to go unnoticed.

The English-only vectors ended up labeling the rows with the wrong English words, unfortunately, making the data they contained meaningless. If you use the <a href="http://conceptnet.s3.amazonaws.com/downloads/2017/numberbatch/numberbatch-17.04.txt.gz">multilingual version</a>, it was and still is fine.

If you use the English-only vectors, we have a new Numberbatch download, <a href="http://conceptnet.s3.amazonaws.com/downloads/2017/numberbatch/numberbatch-en-17.04b.txt.gz">version 17.04b</a>, that should fix the problem.

I apologize for the erroneous data, and for the setback this may have caused for anyone who is just trying to use the best word vectors they can. Thank you to the users on the <a href="https://groups.google.com/forum/#!forum/conceptnet-users">conceptnet-users mailing list</a> who drew my attention to the problem.</p></body></html>