<html><body><a href="https://github.com/LuminosoInsight/python-ftfy">ftfy</a> is Luminoso's open-source Unicode-fixing library for Python.

Luminoso's biggest open-source project is ConceptNet, but we also use this blog to provide updates on our other open-source projects. And among these projects, ftfy is certainly the most widely used. It solves a problem a lot of people have with "no faffing about", as a grateful e-mail I received put it.

When you use the <code>ftfy.fix_text()</code> function, it detects and fixes such problems as mojibake (text that was decoded in the wrong encoding), accidental HTML escaping, curly quotes where you expected straight ones, and so on. (You can also selectively disable these fixes, or run them as separate functions.)

Here's an example that fixes some multiply-mangled Unicode that I actually found on the Web:

<pre>&gt;&gt;&gt; print(ftfy.fix_text("&amp;macr;\\_(ãƒ„)_/&amp;macr;"))
¯\_(ツ)_/¯
</pre>

Another example, from a Twitter-bot gone wrong:

<pre>&gt;&gt;&gt; print(ftfy.fix_text("#╨┐╤Ç╨░╨▓╨╕╨╗╤î╨╜╨╛╨╡╨┐╨╕╤é╨░╨╜╨╕╨╡"))
#правильноепитание
</pre>

So we're proud to present two new releases of ftfy, versions 4.4 and 5.0. Let's start by talking about the big change:

<img class="alignnone size-full wp-image-528" src="/2017/03/ftfy-red-button.png" alt="A control panel labeled in Polish, with a big red button with the text 'Drop Python 2 support' overlaid." width="1321" height="882"> Photo credit: "The Big Red Button" by <a href="https://www.flickr.com/photos/wlodi/">włodi</a>, used under the CC-By-SA 2.0 license

That's right: as of version 4.4, ftfy is better at dealing with encodings of Eastern European languages! After all, sometimes your text is in Polish, like the labels on this very serious-looking control panel. Or maybe it's in Czech, Slovak, Hungarian, or a language with similar accented letters.

Before Unicode, people would handle these alphabets using a single-byte encoding designed for them, like Windows-1250, which would be incompatible with other languages. In that encoding, the photographer's name is the byte string <code>w\xb3odi</code>. But now the standard encoding of the Web is UTF-8, where the same name is <code>w\xc5\x82odi</code>.

The encoding errors you might encounter due to mixing these up used to be underrepresented in the test data I collected. You might end up with the name looking like "wĹ‚odi" and ftfy would just throw up its hands like <code>&amp;macr;\\_(ãƒ„)_/&amp;macr;</code>. But now it understands what happened to that name and how to fix it.

Oh, but what about that text I photoshopped onto the button?

<img class="alignnone size-large wp-image-542" src="/2017/03/ftfy-red-button-small.png" alt="The same image, cropped to just the 'Drop Python 2 support' button." width="400" height="344">

Yeah, I was pulling your leg a bit by talking about the Windows-1250 thing first.

ftfy 5.0 is the same as ftfy 4.4, but it drops support for Python 2. It also gains some tests that we're happier to not have to write for both versions of Python. Depending on how inertia-ful your use of Python is, this may be a big deal to you.

<h2>Three at last!</h2>

Python 3 has a string type that's a pretty good representation of Unicode, and it uses it consistently throughout its standard library. It's a great language for describing Unicode and how to fix it. It's a great language for text in general. But until now, we've been writing ftfy in the unsatisfying language known as "Python 2+3", where you can't take advantage of anything that's cleaner in Python 3 because you still have to do it the Python 2.7 way also.

So, following the plan we announced in April 2015, we released two versions at the same time. They do the same thing, but ftfy 5.0 gets to have shorter, simpler code.

It seems we even communicated this to ftfy's users successfully. Shortly after ftfy 5.0 appeared on PyPI, the bug report we received wasn't about where Python 2 support went, it was about a regression introduced by the new heuristics. (That's why 4.4.1 and 5.0.1 are out already.)

There's more I plan to do with ftfy, especially fixing more kinds of encoding errors, as summarized by <a href="https://github.com/LuminosoInsight/python-ftfy/issues/18">issue #18</a>. It'll be easier to make it happen when I can write the fix in a single language.

But if you're still on Python 2 -- possibly due to forces outside your control -- I hope I've left you with a pretty good option. Thousands of users are content with ftfy 4, and it's not going away.

<h2>One more real-world example</h2>

<pre>&gt;&gt;&gt; from ftfy.fixes import fix_encoding_and_explain
&gt;&gt;&gt; fix_encoding_and_explain("NapĂ\xadĹˇte nĂˇm !")
('Napíšte nám !',
 [('encode', 'sloppy-windows-1250', 2), ('decode', 'utf-8', 0)])</pre></body></html>