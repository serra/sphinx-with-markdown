# Beni's very comprehensive answer

> This is Beni's answer, taken from [StackOverflow](http://stackoverflow.com/a/2487862/322283)

The "proper" way to do that would be to write a [docutils parser][1] for markdown.  (Plus a Sphinx option to choose the parser.)  The beauty of this would be instant support for all docutils output formats (but you might not care about that, as similar markdown tools already exist for most).  Ways to approach that without developing a parser from scratch:

* You could cheat and write a "parser" that uses [Pandoc][2] to convert markdown to RST and pass that to the RST parser :-).

* You can use an existing markdown->XML parser and transform the result (using XSLT?) to the docutils schema.

* You could take some [existing python markdown parser][3] that lets you define a custom renderer and make it build docutils node tree.

* You could fork the existing RST reader, ripping out everything irrelevant to markdown and changing the different syntaxes ([this comparison](https://gist.github.com/dupuy/1855764) might help)...  
  EDIT: I don't recommend this route unless you're prepared to heavily test it.  Markdown already has too many subtly different dialects and this will likely result in yet-another-one...

**UPDATE:** https://github.com/sgenoud/remarkdown is a markdown reader for docutils.  It didn't take any of the above shortcuts but uses a [Parsley][4] PEG grammar inspired by [peg-markdown][5]. [Doesn't yet][6] support directives.

**UPDATE: https://github.com/rtfd/recommonmark and is another docutils reader, natively supported on ReadTheDocs.**  Derived from remarkdown but uses the [CommonMark-py][7] parser.  Doesn't support directives, but [can convert][8] more or less natural Markdown syntaxes to appropriate structures e.g. list of links to a toctree.  For other needs, an [```` ```eval_rst ```` fenced block][9] lets you embed any rST directive.

----

In **all** cases, you'll need to invent extensions of Markdown to represent [Sphinx directives and roles][10].  While you may not need all of them, some like `.. toctree::` are essential.  
This I think is the hardest part.  reStructuredText before the Sphinx extensions was already richer than markdown.  Even heavily extended markdown, such as [pandoc's][11], is mostly a subset of rST feature set.  That's a lot of ground to cover!

Implementation-wise, the easiest thing is adding a generic construct to express any docutils role/directive.  The obvious candidates for syntax inspiration are:

  - Attribute syntax, which pandoc and some other implementations already allow on many inline and block constructs.  For example `` `foo`{.method} `` -> `` `foo`:method: ``.
  - HTML/XML.  From `<span class="method">foo</span>` to the kludgiest approach of just inserting docutils internal XML!
  - Some kind of YAML for directives?

But such a generic mapping will not be the most markdown-ish solution...
Currently most active places to discuss markdown extensions are https://groups.google.com/forum/#!topic/pandoc-discuss, https://github.com/scholmd/scholmd/

This also means you can't just reuse a markdown parser without extending it somehow.  Pandoc again lives up to its reputation as the swiss army knife of document conversion by supporting [custom filtes][12].  (In fact, if I were to approach this I'd try to build a generic bridge between docutils readers/transformers/writers and pandoc readers/filters/writers.  It's more than you need but the payoff would be much wider than just sphinx/markdown.)

----

Alternative crazy idea: instead of extending markdown to handle Sphinx, extend reStructuredText to support (mostly) a superset of markdown!  The beauty is you'll be able to use any Sphinx features as-is, yet be able to write most content in markdown.

There is already [considerable syntax overlap][13]; most notably link syntax is incompatible.  I think if you add support to RST for markdown links, and `###`-style headers, and change default `` `backticks` `` role to literal, and maybe change indented blocks to mean literal (RST supports `> ...` for quotations nowdays), you'll get something usable that supports most markdown.


  [1]: http://docutils.sourceforge.net/docs/dev/hacking.html#parsing-the-document
  [2]: http://johnmacfarlane.net/pandoc/
  [3]: http://lepture.com/en/2014/markdown-parsers-in-python
  [4]: https://pypi.python.org/pypi/Parsley
  [5]: https://github.com/jgm/peg-markdown
  [6]: https://github.com/sgenoud/remarkdown/issues/2
  [7]: https://github.com/rolandshoemaker/
  [8]: https://recommonmark.readthedocs.org/en/latest/auto_structify.html
  [9]: https://recommonmark.readthedocs.org/en/latest/auto_structify.html#embed-restructuredtext
  [10]: http://sphinx-doc.org/markup/index.html
  [11]: http://johnmacfarlane.net/pandoc/README.html#pandocs-markdown
  [12]: http://johnmacfarlane.net/pandoc/scripting.html
  [13]: https://gist.github.com/dupuy/1855764