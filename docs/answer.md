# Marijn's Answer

> This is my answer, taken from [StackOverflow](http://stackoverflow.com/a/33797841/322283)


You can use Markdown and reStructuredText in the same Sphinx project. How to do this is succinctly documented on [Read The Docs]. Install recommonmark (`pip install recommonmark`) and then edit `conf.py`:

    from recommonmark.parser import CommonMarkParser
    
    source_parsers = {
        '.md': CommonMarkParser,
    }
    
    source_suffix = ['.rst', '.md']



 [Read The Docs]: http://docs.readthedocs.org/en/latest/getting_started.html#in-markdown
 [beni]: http://stackoverflow.com/a/2487862/322283