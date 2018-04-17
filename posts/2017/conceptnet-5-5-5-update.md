<html><body><p>ConceptNet 5.5.5 is out, and it's running on <a href="http://conceptnet.io">conceptnet.io</a>. The <code>version5.5</code> tag in Git has been updated to point to this version. Here's what's new.

</p><h2>Changelog</h2>

Data changes:

<ul>
<li>Uses ConceptNet Numberbatch 17.06, which incorporates <a href="http://blog.conceptnet.io/2017/04/24/conceptnet-numberbatch-17-04-better-less-stereotyped-word-vectors/">de-biasing</a> to avoid harmful stereotypes being encoded in its word representations.</li>
<li>Fixed a glitch in retrofitting, where terms in ConceptNet that were two steps removed from any term that existed in one of the existing word-embedding data sources were all being assigned the same meaningless vector. They now get vectors that are propagated (after multiple steps) from terms that do have existing word embeddings, as intended.</li>
<li>Filtered some harmful assertions that came from disruptive or confused Open Mind Common Sense contributors. (Some of them had been filtered before, but changes to the term representation had defeated the filters.)</li>
<li>Added a new source of input word embeddings, created at Luminoso by running a multilingual variant of fastText over OpenSubtitles 2016. This provides a source of real-world usage of non-English words.</li>
</ul>

Build process changes:

<ul>
<li>We measured the amount of RAM the build process requires at its peak to be 30 GB, and tested that it completes on a machine with 32 GB of RAM. We updated the Snakefile to reflect these requirements and to use them to better plan which tasks to run in parallel.</li>
<li>The build process starts by checking for some requirements (having enough RAM, enough disk space, and a usable PostgreSQL database), and exits early if they aren't met, instead of crashing many hours later.</li>
<li>The tests have been organized into tests that can be run before building ConceptNet, tests that can be run after a small example build, and tests that require the full ConceptNet. The first two kinds of tests are run automatically, in the right sequence, by the <code>test.sh</code> script.</li>
<li><code>test.sh</code> and <code>build.sh</code> have been moved into the top-level directory, where they are more visible.</li>
</ul>

Library changes:

<ul>
<li>Uses the <code>marisa-trie</code> library to speed up inferring vectors for out-of-vocabulary words.</li>
<li>Uses the <code>annoy</code> library to suggest nearest neighbors that map a larger vocabulary into a smaller one.</li>
<li>Depends on a specific version of <code>xmltodict</code>, because a breaking change to <code>xmltodict</code> managed to break the build process of many previous versions of ConceptNet.</li>
<li>The <code>cn5-vectors evaluate</code> command can evaluate whether a word vector space contains gender biases or ethnic biases.</li>
</ul>

<h2>Understanding our version numbers</h2>

Version numbers in modern software are typically described as <em>major.minor.micro</em>. ConceptNet's version numbers would be better described as <em>mega.major.minor</em>. Now that all the version components happen to be 5, I'll explain what they mean to me.

The change from 5.5.4 to 5.5.5 is a "minor" change. It involves important fixes to the data, but these fixes don't affect a large number of edges or significantly change the vocabulary. If you are building research on ConceptNet and require stable results, we suggest building a particular version (such as 5.5.4 or 5.5.5) from its <a href="https://github.com/commonsense/conceptnet5/wiki/Running-your-own-copy">Docker</a> container, as a "minor" change could cause inconsistent results.

The change from 5.4 to 5.5 was a "major" change. We changed the API format somewhat (hopefully with a smooth transition), we made significant changes to ConceptNet's vocabulary of terms, we added new data sources, and we even changed the domain name where it is hosted. We're working on another "major" update, version 5.6, that incorporates new data sources again, though I believe the changes will not be as sweeping as the 5.5 update.

The change from ConceptNet 4 to ConceptNet 5 (six years ago) was a "mega" change, a thorough rethinking and redesign of the project, keeping things that worked and discarding things that didn't, which is not well described by software versions. The appropriate way to represent it in Semantic Versioning would probably be to start a new project with a different name.

Don't worry, I have no urge to make a ConceptNet 6 anytime soon. ConceptNet 5 is doing great.

The word vectors that ConceptNet uses in its <a href="https://github.com/commonsense/conceptnet5/wiki/API#looking-up-related-terms">relatedness API</a> (which are also distributed separately as <a href="http://github.com/commonsense/conceptnet-numberbatch">ConceptNet Numberbatch</a>) are recalculated for every version, even minor versions. The results you get from updating to new vectors should get steadily more accurate, unless your results depended on the ability to represent <a href="https://arxiv.org/abs/1607.06520">harmful stereotypes</a>.

You can't mix old and new vectors, so any machine-learning model needs to be rebuilt to use new vectors. This is why we gave ConceptNet Numberbatch a version numbering scheme that is entirely based on the date (vectors computed in June 2017 are version 17.06).</body></html>
