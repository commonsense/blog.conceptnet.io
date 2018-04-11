<html><body>

<a href="https://github.com/LuminosoInsight/python-ftfy" target="_blank" rel="noopener noreferrer">ftfy</a> is a Python tool that takes in bad Unicode and outputs good Unicode. I developed it because we really needed it at <a href="http://www.luminoso.com" target="_blank" rel="noopener noreferrer">Luminoso</a> -- the text we work with can be damaged in several ways by the time it gets to us. It's become our most popular open-source project by far, as many other people have the same itch that we're scratching.

The coolest thing that ftfy does is to fix mojibake -- those mix-ups in encodings that cause the word <code>más</code> to turn into <code>mÃ¡s</code> or even <code>mÃƒÂ¡s</code>. (I'll recap why this happens and how it can be reversed below.) Mojibake is often intertwined with other problems, such as un-decoded HTML entities (<code>más</code>), and ftfy fixes those as well. But as we worked with the ftfy 3 series, it gradually became clear that the default settings were making some changes that were unnecessary, and from time to time they would actually get in the way of the goal of cleaning up text.

ftfy 4 includes interesting new fixes to creative new ways that various software breaks Unicode. But it also aims to change <em>less</em> text that doesn't need to be changed. This is the big change that made us increase the major version number from 3 to 4, and it's fundamentally about Unicode normalization. I'll discuss this change below under the heading "Normalization".

<h2>Mojibake and why it happens</h2>

Mojibake is what happens when text is written in one encoding and read as if it were a different one. It comes from the Japanese word "•¶Žš‰»‚¯" -- no, sorry, "文字化け" -- meaning "character corruption". Mojibake turns everything but basic ASCII characters into nonsense.

Suppose you have a word such as "más". In UTF-8 -- the encoding <a href="http://w3techs.com/technologies/overview/character_encoding/all" target="_blank" rel="noopener noreferrer">used by the majority of the Internet</a> -- the plain ASCII letters "m" and "s" are represented by the familiar single byte that has represented them in ASCII for 50 years. The letter "á", which is not ASCII, is represented by two bytes.

```http://blog.emojipedia.org/apple-2015-emoji-changelog-ios-os-x

curl http://example.com/api/data.txt | ftfy | sort | uniq -c

```

The details of all the changes can be found, of course, in the <a href="https://github.com/LuminosoInsight/python-ftfy/blob/20b6698a7f2cc565bd240121fba586cf6c6f3bc5/CHANGELOG.md" target="_blank" rel="noopener noreferrer">CHANGELOG</a>.

Has ftfy solved a problem for you? Have you stumped it with a particularly bizarre case of mojibake? Let us know in the comments or <a href="https://twitter.com/luminosoinsight" target="_blank" rel="noopener noreferrer">on Twitter</a>.</body></html>
