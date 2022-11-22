# Marijn's Answer

> This is my answer, taken from [StackOverflow](http://stackoverflow.com/a/33797841/322283)

You can use Markdown and reStructuredText in the same Sphinx project. 
How to do this is succinctly documented in the [Sphinx documentation]. 

Install `myst-parser` (`pip install myst-parser`) and then edit `conf.py`:

    # simply add the extension to your list of extensions
    extensions = ['myst_parser']
    
    source_suffix = ['.rst', '.md']

---

I've created a small example project [on Github (serra/sphinx-with-markdown)](https://github.com/serra/sphinx-with-markdown) demonstrating how (and that) it works. It uses Sphinx version 3.5.4 and myst-parser version 0.14.0.


 [Sphinx documentation]: https://www.sphinx-doc.org/en/master/usage/markdown.html


## Pre-april 2021 

You can use Markdown and reStructuredText in the same Sphinx project. How to do this is succinctly documented on [Read The Docs]. 

Install recommonmark (`pip install recommonmark`) and then edit `conf.py`:

    from recommonmark.parser import CommonMarkParser
    
    source_parsers = {
        '.md': CommonMarkParser,
    }
    
    source_suffix = ['.rst', '.md']

---

I've created a small example project [on Github (serra/sphinx-with-markdown)](https://github.com/serra/sphinx-with-markdown) demonstrating how (and that) it works. It uses CommonMark 0.5.4 and recommonmark 0.4.0.


 [Read The Docs]: http://docs.readthedocs.org/en/latest/getting_started.html#in-markdown
 [beni]: http://stackoverflow.com/a/2487862/322283