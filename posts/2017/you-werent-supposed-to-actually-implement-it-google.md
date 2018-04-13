Last month, I wrote a blog post warning about how, if you follow popular trends in NLP, you can easily accidentally make <a href="http://blog.conceptnet.io/2017/07/13/how-to-make-a-racist-ai-without-really-trying/">a classifier that is pretty racist</a>. To demonstrate this, I included the very simple code, as a "cautionary tutorial".

The post got a fair amount of reaction. Much of it positive and taking it seriously, so thanks for that. But eventually I heard from some detractors. Of course there were the fully expected "I'm not racist but what if racism is correct" retorts that I knew I'd have to face. But there were also people who couldn't believe that anyone does NLP this way. They said I was talking about a non-problem that doesn't show up in serious machine learning, or projecting my own bad NLP ideas, or something.

Well. Here's <a href="http://perspectiveapi.com/">Perspective API</a>, made by an offshoot of Google. They believe they are going to use it to fight "toxicity" online. And by "toxicity" they mean "saying anything with negative sentiment". And by "negative sentiment" they mean "whatever word2vec thinks is bad". It works exactly like the hypothetical system that I cautioned against.

On this blog, we've just <em>looked</em> at what word2vec (or GloVe) thinks is bad. It includes black people, Mexicans, Islam, and given names that don't usually belong to white Americans. You can <em>actually type my examples into Perspective API</em> and it will <em>actually</em> <em>respond</em> that the ones that are less white-sounding are more "likely to be perceived as toxic".

<ul>
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

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">It&#39;s 🤣 to poke around and see what biases the system picked up from the training data. 😰 to think about actual applications, though. <a href="https://t.co/VJ9y9yxz2D">pic.twitter.com/VJ9y9yxz2D</a></p>&mdash; Dan Luu (@danluu) <a href="https://twitter.com/danluu/status/896177697285603329?ref_src=twsrc%5Etfw">August 12, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I have previously written positive things about researchers at Google who are looking at approaches to de-biasing AI, such as their blog post on <a href="https://research.googleblog.com/2016/10/equality-of-opportunity-in-machine.html">Equality of Opportunity in Machine Learning</a>.

But Google is a big place. It contains multitudes. And it seems it contains a subdivision that will do the wrong thing, which other Googlers <em>know</em> is the wrong thing, because it's easy.

Google, you made a very bad investment. (That sentence is 61% toxic, by the way.)

## Epilogue

As I update this post in April 2018, I've had some communication with the Perspective API team and learned some more details about it.

Some details of this post were incorrect, based on things I assumed when looking at Perspective API from outside. For example, Perspective API does not literally build on word2vec. But the end result is the same: it learns the same biases that word2vec learns anyway.

In September 2017, [Violet Blue wrote an exposé of Perspective API](https://www.engadget.com/2017/09/01/google-perspective-comment-ranking-system/) for Engadget. Despite the details that I had wrong, the Engadget article confirms that the system really is that bad, and provides even more examples.

Perspective API has changed their online demo to lower toxicity scores across the board, without fundamentally changing the model. Text with a score under a certain threshold is now labeled as "not toxic". I believe this remedy could be described technically as "weak sauce".

The Perspective API team claims that their system has no inherent bias against non-white names, and that the higher toxicity scores that appear for names such as "DeShawn" is an artifact of how they handle out-of-vocabulary words. All the names that are typical for white Americans are in-vocabulary. Make of that what you will.

The Perspective API team continues to promote their product, such as via hackathons and TED talks. Users of the API are not warned of its biases, except for a generic warning that could apply to any AI system, saying that users should manually review its results. It is still sometimes held up as a *positive* example of fighting toxicity with NLP, misleading lay audiences into thinking that present NLP has a solution to toxicity.
