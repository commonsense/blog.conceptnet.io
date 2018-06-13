.. title: ConceptNet's strong performance at SemEval 2018
.. slug: conceptnet-at-semeval-2018
.. date: 2018-06-14 13:00:00 UTC-04:00
.. tags: ConceptNet
.. category: 
.. link: 
.. description: 
.. type: text

At the beginning of June, we went to the NAACL conference and the SemEval workshop.
SemEval is a yearly event where NLP systems are compared head-to-head on semantic
tasks, and how they perform on unseen test data.

I like to submit to SemEval because I see it as the NLP equivalent of
pre-registered studies. You know the results are real; they're not
cherry-picked positive results, and they're not repeatedly tuned to the same
test set. SemEval provides valuable information on which semantic techniques
actually work well on new data.

Recently, SemEval has been a compelling demonstration of why ConceptNet is
important in semantics. The results of multiple tasks have shown the advantage
of using a knowledge graph, particularly ConceptNet, and not assuming that
a distributional representation such as word2vec will learn everything there
is to learn.

Last year we got the top score (by a wide margin) in the SemEval task that we
entered using ConceptNet Numberbatch (pre-trained word vectors built from
ConceptNet). I was wondering if we had really made an impression with this
result, or if the field was going to write it off as a fluke and go on as it
was.

We made an impression! This year at SemEval, there were many systems using
ConceptNet, not just ours. Let's look at these tasks.

## Story understanding

[Task 11: Machine Comprehension Using Commonsense Knowledge][task11-desc] is a
task where your NLP system reads a story and then answers some simple questions
that test its comprehension. There are many NLP evaluations that involve
reading comprehension, but many of them are susceptible to shallow strategies
where the machine just learns to parrot key phrases from the text. The
interesting twist in this one is that about half of the answers are not present
in the text, but are meant to be inferred using common sense knowledge.

Here's an example from [the task paper][task11-desc], by Simon Ostermann et al.:

> **Text**: It was a long day at work and I decided to stop at the gym before
> going home.  I ran on the treadmill and lifted some weights. I decided I
> would also swim a few laps in the pool. Once I was done working out, I went
> in the locker room and stripped down and wrapped myself in a towel. I went
> into the sauna and turned on the heat. I let it get nice and steamy. I sat
> down and relaxed. I let my mind think about nothing but peaceful, happy
> thoughts. I stayed in there for only about ten minutes because it was so
> hot and steamy. When I got out, I turned the sauna off to save energy and
> took a cool shower. I got out of the shower and dried off. After that, I
> put on my extra set of clean clothes I brought with me, and got in my car
> and drove home.
> 
> **Q1**: Where did they sit inside the sauna?
>     
> (a) on the floor  
> (b) on a bench
> 
> **Q2**: How long did they stay in the sauna?
> 
> (a) about ten minutes  
> (b) over thirty minutes
 
Q1 is not just asking for a phrase to be echoed from the text. It requires some
common sense knowledge, such as that saunas contain benches, that benches are
meant for people to sit on, and that people will probably sit on a bench in
preference to the floor.

It's no wonder that the top system, from [Yuanfudao Research][yuanfudao], made
use of ConceptNet and got a boost from its common sense knowledge. Their
architecture was an interesting one I haven't seen before -- they queried the
ConceptNet API for what relations existed between words in the text, the
question, and the answer, and used the results they got as inputs to their
neural net.

I hadn't heard about this system before the workshop. It was quite satisfying
to see ConceptNet win at a difficult task without any effort from us!

[yuanfudao]: https://arxiv.org/pdf/1803.00191.pdf

## Telling word meanings apart

Our entry this year was for [Task 10: Capturing Discriminative
Attributes][task10-desc], a task about recognizing difference between words.
Our system used ConceptNet Numberbatch in combination with four other
resources, and took second place at the task.

Our system is best described by our poster, which we brought to the event on a
frighteningly large 6'×4' poster that loomed over everyone and threatened to
tip over the two tiny poster stands holding it up. (We've now learned that
A0-sized posters are a vague academic standard, and if an event asks for a
larger poster, you should ignore them, because nobody expects a poster larger
than A0.) Anyway, now you can read our poster from the comfort of your Web
browser:

<a href="/2018/06/naacl2018-poster.pdf">
<img src="/2018/06/naacl2018-poster.png" alt="A rendering of our poster. The link leads to a PDF version.">
</a>

In their [summary paper][task10-desc], the task organizers (Alicia Krebs,
Alessandro Lenci, and Denis Paperno) highlight the fact that systems that
used knowledge bases performed much better than those that didn't. Here's
a table of the results, which we've adapted from their paper and annotated
with the largest knowledge base used by each entry:

<table class="table" style="width: 50%; font-size: 90%;">
<thead><tr>
<th align="right">Rank</th>
<th>Team</th>
<th align="right">Score</th>
<th>Knowledge base</th>
</tr></thead>
<tbody>
<tr style="background-color: #ffc">
<td align="right">1</td>
<td>SUNNYNLP</td>
<td align="right">0.75</td>
<td>Probase</td>
</tr>
<tr style="background-color: #cfc">
<td align="right">2</td>
<td>Luminoso</td>
<td align="right">0.74</td>
<td>ConceptNet</td>
</tr>
<tr>
<td align="right">3</td>
<td>BomJi</td>
<td align="right">0.73</td>
<td></td>
</tr>
<tr style="background-color: #cfc">
<td align="right">3</td>
<td>NTU NLP</td>
<td align="right">0.73</td>
<td>ConceptNet</td>
</tr>
<tr style="background-color: #cfc">
<td align="right">5</td>
<td>UWB</td>
<td align="right">0.72</td>
<td>ConceptNet</td>
</tr>
<tr style="background-color: #cfc">
<td align="right">6</td>
<td>ELiRF-UPV</td>
<td align="right">0.69</td>
<td>ConceptNet</td>
</tr>
<tr style="background-color: #ffc">
<td align="right">6</td>
<td>Meaning Space</td>
<td align="right">0.69</td>
<td>WordNet</td>
</tr>
<tr style="background-color: #cfc">
<td align="right">6</td>
<td>Wolves</td>
<td align="right">0.69</td>
<td>ConceptNet</td>
</tr>
<tr>
<td align="right">9</td>
<td>Discriminator</td>
<td align="right">0.67</td>
<td></td>
</tr>
<tr style="background-color: #ffc">
<td align="right">9</td>
<td>ECNU</td>
<td align="right">0.67</td>
<td>WordNet</td>
</tr>
<tr>
<td align="right">11</td>
<td>AmritaNLP</td>
<td align="right">0.66</td>
<td></td>
</tr>
<tr>
<td align="right">12</td>
<td>GHH</td>
<td align="right">0.65</td>
<td></td>
</tr>
<tr>
<td align="right">13</td>
<td>ALB</td>
<td align="right">0.63</td>
<td></td>
</tr>
<tr>
<td align="right">13</td>
<td>CitiusNLP</td>
<td align="right">0.63</td>
<td></td>
</tr>
<tr>
<td align="right">13</td>
<td>THU NGN</td>
<td align="right">0.63</td>
<td></td>
</tr>
<tr style="background-color: #ffc">
<td align="right">16</td>
<td>UNBNLP</td>
<td align="right">0.61</td>
<td>WordNet</td>
</tr>
<tr>
<td align="right">17</td>
<td>UNAM</td>
<td align="right">0.60</td>
<td></td>
</tr>
<tr>
<td align="right">17</td>
<td>UMD</td>
<td align="right">0.60</td>
<td></td>
</tr>
<tr style="background-color: #ffc">
<td align="right">19</td>
<td>ABDN</td>
<td align="right">0.52</td>
<td>WordNet</td>
</tr>
<tr>
<td align="right">20</td>
<td>Igevorse</td>
<td align="right">0.51</td>
<td></td>
</tr>
<tr>
<td align="right">21</td>
<td>bicici</td>
<td align="right">0.47</td>
<td></td>
</tr>
<tr>
<td align="right"></td>
<td>human ceiling</td>
<td align="right">0.90</td>
<td></td>
</tr>
<tr>
<td align="right"></td>
<td>word2vec baseline</td>
<td align="right">0.61</td>
<td></td>
</tr>
</tbody>
</table>

The winning system made very effective use of Probase, a hierarchy of
automatically extracted "is-a" statements about noun phrases. Unfortunately,
Probase was never released for non-academic use; it became the Microsoft
Concept Graph, which was recently shut down.

We can see here that five systems used ConceptNet in their solution, and their
various papers describe how ConceptNet provided a boost to their accuracy.

In our own results, we encountered the surprising retrospective result that we could have simplified our system to just
use the ConceptNet Numberbatch embeddings, and no other sources of information,
and it would have done just as well! You can read a bit more about this in
[the poster][poster], and I hope to demonstrate this
simple system in a tutorial post soon.

[poster]: /2018/06/naacl2018-poster.pdf
[task10-desc]: http://aclweb.org/anthology/S18-1117
[task11-desc]: http://www.aclweb.org/anthology/S18-1119
