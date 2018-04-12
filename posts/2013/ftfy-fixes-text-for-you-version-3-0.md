About a year ago, we blogged about how to ungarble garbled Unicode in a post called <a title="Fixing common Unicode mistakes with Python â€” after they’ve been made" href="/posts/2012/fixing-common-unicode-mistakes-with-python-after-theyve-been-made/">Fixing common Unicode mistakes with Python â€” after they’ve been made</a>. Shortly after that, we released the code in a Python package called <a href="https://github.com/LuminosoInsight/python-ftfy">ftfy</a>.

You have almost certainly seen the kind of problem ftfy fixes. Here's <a href="http://isabelcastillo.com/international-characters-encoding-fpdf">a shoutout</a> from a developer who found that her database was full of place names such as "BucureÅŸti, Romania" because of someone else's bug. That's easy enough to fix:

```
pip install ftfy
```

If ftfy is useful to you, we'd love to hear how you're using it. You can reply to the comments here or e-mail us at <a href="mailto:info@luminoso.com">info@luminoso.com</a>.</body></html>
