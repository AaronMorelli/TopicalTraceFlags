===========
reST basics
===========

This is text under the title of the page


First Flags
-----------

2388 : ``Doc2005`` : (Informational)
	920093_: does the blah blah to the blah blah and sometimes also the blah blah. This is a really long string to see how RTD handles wrapping text. Yes indeedy, wrappy wrappy. Eggs and backups-y. 
	Here's the first text of the second descriptive line. CSS1_: Another helpful descriptive statement.
	
	ThirdLink_ | FourthLink_ | FifthLink_

`...`	
	
2389 : ``Doc2016``
	920093_: does the blah blah to the blah blah and sometimes also the blah blah. This is a really long string to see how RTD handles wrapping text. Yes indeedy, wrappy wrappy. Eggs and backups-y. 
	Here's the first text of the second descriptive line. CSS1_: Another helpful descriptive statement.
	
	ThirdLink_ | FourthLink_ | FifthLink_	
	
	
	An indented flag ``Doc2008`` *Info*  
		920093_: more descriptions, dude. CSS2_: Moar tekts.
		
		ThirdLink_ | FourthLink_ | FifthLink_ 
	
	
	

	
	
	
Second Flags
------------

Here is a normal paragraph containing *italic*, **bold**, `interpreted text`, ``inline literals``, 
standalone hyperlinks (http://www.python.org), external hyperlinks (Python_), internal cross-references
(example_), footnote references ([1]_), citation references ([CIT2002]_), substitution pictures 
(|D2014|), and _`inline internal targets`.

Multiple paragraphs are separated by blank lines and are left-aligned.

.. _Python: http://www.python.org

.. [1] A footnote contains body elements, consistently
   indented by at least 3 spaces.
   
.. [CIT2002] Just like a footnote, except the label is
   textual.
   
.. _example:

The "_example" target above points to this paragraph.


- Here's a standard bullet list. This one contains a hyperlink to Python_.
- This is the second item

- This is the third item, with a space between it and the second item (in the source)

- This is the fourth item, and has multiple lines.
	See, a second line!
	
- This is a fifth item, but the paragraph has probs because
the second line isn't indented enough to be equal to the "This" at its start.

#. This is an enumerated list
#. here's the second item. Here's a hyperlink to Python_.


Literal blocks are either indented or line-prefix-quoted blocks,
and indicated with a double-colon ("::") at the end of the
preceding paragraph (right here -->)::

    if literal_block:
        text = 'is left as-is'
        spaces_and_linebreaks = 'are preserved'
        markup_processing = None
		
		
Block quotes consist of indented body elements:

    This theory, that is mine, is mine. Link to Python_

    -- Anne Elk (Miss) Another link to Python_
	
	
Another top-level paragraph.

        This paragraph belongs to a second-level block quote.

    This paragraph belongs to a first-level block quote.  The
    second-level block quote above is inside this first-level
    block quote.



.. _920093: https://support.microsoft.com/en-us/kb/920093
.. _CSS1: https://blogs.msdn.microsoft.com/psssql/2016/11/15/unable-to-drop-a-user-in-a-database/
.. _CSS2: https://blogs.msdn.microsoft.com/psssql/2016/11/15/unable-to-drop-a-user-in-a-database/
.. _ThirdLink: http://www.python.org
.. _FourthLink: http://www.python.org
.. _FifthLink: http://www.python.org
		