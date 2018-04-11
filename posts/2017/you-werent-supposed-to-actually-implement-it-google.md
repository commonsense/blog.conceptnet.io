<html><body><p>Last month, I wrote a blog post warning about how, if you follow popular trends in NLP, you can easily accidentally make <a href="https://blog.conceptnet.io/2017/07/13/how-to-make-a-racist-ai-without-really-trying/">a classifier that is pretty racist</a>. To demonstrate this, I included the very simple code, as a "cautionary tutorial".

The post got a fair amount of reaction. Much of it positive and taking it seriously, so thanks for that. But eventually I heard from some detractors. Of course there were the fully expected "I'm not racist but what if racism is correct" retorts that I knew I'd have to face. But there were also people who couldn't believe that anyone does NLP this way. They said I was talking about a non-problem that doesn't show up in serious machine learning, or projecting my own bad NLP ideas, or something.

Well. Here's <a href="http://perspectiveapi.com/">Perspective API</a>, made by an offshoot of Google. They believe they are going to use it to fight "toxicity" online. And by "toxicity" they mean "saying anything with negative sentiment". And by "negative sentiment" they mean "whatever word2vec thinks is bad". It works exactly like the hypothetical system that I cautioned against.

On this blog, we've just <em>looked</em> at what word2vec (or GloVe) thinks is bad. It includes black people, Mexicans, Islam, and given names that don't usually belong to white Americans. You can <em>actually type my examples into Perspective API</em> and it will <em>actually</em> <em>respond</em> that the ones that are less white-sounding are more "likely to be perceived as toxic".

</p><ul>
    <li>"Hello, my name is Emily" is supposedly <strong>4%</strong> likely to be "toxic". Similar results for "Susan", "Paul", etc.</li>
    <li>"Hello, my name is Shaniqua" ("Jamel", "DeShawn", etc.): <strong>21%</strong> likely to be toxic.</li>
    <li>"Let's go get Italian food": <strong>9%</strong>.</li>
    <li>"Let's go get Mexican food": <strong>29%</strong>.</li>
</ul>

Here are two more examples I didn't mention before:

<ul>
    <li>"Christianity is a major world religion": <strong>37%</strong>. Okay, maybe things can get heated when religion comes up at all, but compare:</li>
    <li>"Islam is a major world religion": <strong>66% toxic</strong>.</li>
</ul>

I've heard about Perspective API from many directions, but my proximate source is this Twitter thread by Dan Luu, who has his own examples:

https://twitter.com/danluu/status/896177697285603329

I have previously written positive things about researchers at Google who are looking at approaches to de-biasing AI, such as their blog post on <a href="https://research.googleblog.com/2016/10/equality-of-opportunity-in-machine.html">Equality of Opportunity in Machine Learning</a>.

But Google is a big place. It contains multitudes. And it seems it contains a subdivision that will do the wrong thing, which other Googlers <em>know</em> is the wrong thing, because it's easy.

Google, you made a very bad investment. (That sentence is 61% toxic, by the way.)</body></html>