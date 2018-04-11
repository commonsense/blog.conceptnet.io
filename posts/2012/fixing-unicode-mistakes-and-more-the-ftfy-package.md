<html><body>
There's been a great response to my earlier post, <a title="Fixing common Unicode mistakes with Python â€” after they’ve been made" href="https://conceptnetblog.wordpress.com/2016/05/19/fixing-common-unicode-mistakes-with-python-ae-after-theyve-been-made/" target="_blank" rel="noopener noreferrer">Fixing common Unicode mistakes with Python</a>. This is clearly something that people besides me needed. In fact, someone already made the code into a web site, at <a title="fixencoding.com" href="http://fixencoding.com/">fixencoding.com</a>. I like the favicon.

I took the suggestion to split the code into a new standalone package. It's now called <strong><a title="python-ftfy" href="http://github.com/LuminosoInsight/python-ftfy">ftfy</a></strong>, standing for "fixes text for you". You can install it with <em>pip install ftfy</em>.

I observed that I was doing interesting things with Unicode in Python, and yet I wasn't doing it in Python 3, which basically makes me a terrible person. ftfy is now compatible with both Python 2 and Python 3.

Something else amusing happened: At one point, someone edited the previous post and WordPress barfed HTML entities all over its text. All the quotation marks turned into ", for example. So, for a bit, that post was setting a terrible example about how to handle text correctly!

I took that as a sign that I should expand <em>ftfy</em> so that it also decodes HTML entities (though it will leave them alone in the presence of HTML tags). While I was at it, I also made it turn curly quotes into straight ones, convert Windows line endings to Unix, normalize Unicode characters to their canonical forms, strip out terminal color codes, and remove miscellaneous control characters. The original <em>fix_bad_unicode</em> is still in there, if you just want the encoding fixer without the extra stuff.</body></html>
