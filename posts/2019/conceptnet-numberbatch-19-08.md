.. title: ConceptNet Numberbatch 19.08
.. slug: conceptnet-numberbatch-19-08
.. date: 2019-08-01 15:22:35 UTC-04:00
.. tags: ConceptNet, Releases, Word embeddings, NLP fairness
.. category:
.. link:
.. description:
.. type: text

<img src="/2019/08/numberbatch-logo.png" alt="ConceptNet Numberbatch logo, featuring an otter">

It's been a while since we made a release of ConceptNet Numberbatch. Here, [have some
new word embeddings][numberbatch].

[ConceptNet Numberbatch][numberbatch] is our whimsical double-dactyl name for
pre-computed word embeddings built using ConceptNet and distributional
semantics.  The things we've been doing with it at
[Luminoso](http://luminoso.com) have benefited from some improvements we've
made in the last two years.

[numberbatch]: https://github.com/commonsense/conceptnet-numberbatch

The last release we announced was in 2017. Since then, we made a few
improvements for SemEval 2018, when we demonstrated how to [distinguish
attributes using ConceptNet](/posts/2018/distinguishing-attributes-using-conceptnet/).

But meanwhile, outside of Luminoso, we've also seen some great things being built with what
we released:

- Alex Lew's [Robot Mind Meld](http://robotmindmeld.com/) uses ConceptNet Numberbatch to play a cooperative improv
  game.

- Tsun-Hsien Tang and others showed how Numberbatch can be combined with image recognition
  to better [retrieve images of daily life](http://ceur-ws.org/Vol-2125/paper_124.pdf).

- Sophie Siebert and Frieder Stolzenburg developed
  [CoRg](http://corg.hs-harz.de/), a reasoning / story understanding system
  that combines Numberbatch word embeddings with theorem proving, a combination
  I wouldn't have expected to see at all.

I hope we can see more projects like this by releasing our improvements to Numberbatch.


## Expanding the vocabulary

We added a step to the build process of ConceptNet Numberbatch called
"propagation".  This makes it easier to use Numberbatch to represent a larger
vocabulary of words, especially in languages with more inflections than
English.

Previously, there were a lot of terms that we didn't have vectors for,
especially word forms that aren't the most commonly-observed form. We had to
rely on the out-of-vocabulary (OOV) strategy to handle these words, by looking
up their neighboring terms in ConceptNet that did have vectors. This strategy
was hard to implement, because it required having access to the ConceptNet
graph at all times.

I know that many projects that attempted to use Numberbatch simply skipped the
OOV strategy, so any word that wasn't directly in the vocabulary just couldn't
be represented, and this led to suboptimal results.

With the "propagation" step, we pre-compute the vectors for more words,
especially forms of known words.

This increases the vocabulary size and the space required to use Numberbatch,
but leaves us with a simple, fast OOV strategy that doesn't need to refer to
the whole ConceptNet graph. And it should improve the results greatly for users
of Numberbatch who aren't using an OOV strategy at all.


## New results on fairness

Word embeddings are a useful tool for a lot of NLP tasks, but by now we've seen
lots of evidence of a risk they carry: when they capture word meanings from the
ways we use words, they also capture harmful biases and stereotypes.  A clear,
up-to-date paper on this is ["What are the biases in my word
embedding?"](https://arxiv.org/pdf/1812.08769.pdf), by Nathaniel Swinger et al.

It's important to do what we can to mitigate that. Machine learning involves
lots of ethical issues, and we can't solve them all while not knowing how
you're even going to use our word embeddings, but we can at least try not to
publish something that makes the ethical problems worse. So one of the steps in
building ConceptNet Numberbatch is *algorithmic de-biasing* that tries to
identify and mitigate these biases.

(If you want to point out that algorithmic de-biasing is insufficient to solve
the problem, you are very right, but that doesn't mean we shouldn't do it.)

Chris Sweeney and Maryam Najafian published [a new framework for assessing
fairness in word embeddings](https://www.aclweb.org/anthology/P19-1162). Their
framework doesn't assume that biases are necessarily binary (men vs. women,
white vs. black) or can be seen in a linear projection of the embeddings, as
previous metrics did. This assessment comes out looking pretty good for
Numberbatch, which associates nationalities and religions with sentiment words
more equitably than other embeddings.

Please note that you can't be assured that your AI project is ethical just
because it has one fairer-than-usual component in it. We have not solved AI
ethics. You still need to test for harmful effects in the outputs of your
actual system, as well as making sure that its inputs are collected ethically.


## Links

You can find download links and documentation for the new version on the
[conceptnet-numberbatch GitHub page](https://github.com/commonsense/conceptnet-numberbatch).

The Python code that builds the embeddings is in `conceptnet5.vectors`, part of
the [conceptnet5 repository][conceptnet5].

[conceptnet5]: https://github.com/commonsense/conceptnet5
