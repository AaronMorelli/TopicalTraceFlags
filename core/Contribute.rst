=================
How to Contribute
=================

Topical Trace Flags is hosted on GitHub_, and operates under the assumption that anyone in the SQL
Server community should be able to add value to the repository in an easy way and receive credit
for doing so. The repository is "coded" via the reStructuredText markup language, but you don't have
to learn this markup to make changes. Even submitters of pull requests can usually just
copy an existing flag's content and then change it, mimicking the various markup tags from the copied text.

To request a change, first search the repo using the Search textbox in the upper-left window. [Annoyingly] you 
apparently must be on the root page of the repo for search to check all sub-pages, so click the "TopicalTraceFlags"
Home button in the upper-left first. You can also use the callout box in the lower left corner 
of the browser or this `search`_ link.

Then, review the search results and confirm which page holds the full definition for the flag (versus the pages
that only hold "SeeAlso" references to the flag). Determine the change(s) you want made, ensuring they comply 
with the `Guidelines for changes`_ section below. 

Once you understand the flag's existing content (or that the flag is completely new), you have several options: 
1) non-GitHub methods like email or Twitter, 2) GitHub issues, or 3) GitHub pull requests.

Submitting through email or Twitter
-----------------------------------
You can contact the current project maintainer (Aaron Morelli) via Twitter_ or by email: aaron no-intervening-characters morelli at zoho daught com.
Please include the following information:

- The flag number
- One or more important links to the flag (see guidelines below). Feel free to send all links you're aware of and the maintainer will
  decide which are valuable enough to include in the repo.
- (optional) How you would prefer to be identified as a contributor (e.g. your email address, Twitter handle, GitHub, etc).
  If omitted, we'll just use the method with which you contacted us.
  
We'll generate a pull request in the repository specifically for your submitted content, and in the comments for that PR identify
you as the submitter. Additionally, the reStructuredText source code will contain a reST comment (not visible to the end user) 
like so::

	.. Submitted by: (Twitter) @FlagSubmitter
	
	
Creating a GitHub Issue
-----------------------
If you have a GitHub account, you may find it easier (and more open-source-y) to create either an issue or a pull request.
(You could do both if you really want.) To create an issue, once you find the page that should be edited from the search you conducted 
above (the page that holds the full def for the flag):

1. Click on the "Edit on GitHub" button in the upper right of the ReadTheDocs version of the page. This takes you to GitHub, 
   specifically to the reStructuredText (.rst) file that corresponds to the web page you just left.
2. Click the "Copy path" button near the upper right.
3. Click the "Issues" tab near the upper left, and then click "New issue"
4. Enter a short description (including the flag number) in the title, and in the comment box, paste the 
   filepath you just copied and any links you have for this flag. If you want to specify (or correct) 
   text for the description of the flag, use quotation marks to specify exactly what you want.
5. Submit the issue.

(For more info see GitHub's help on `submitting an issue`_).

We'll generate a pull request in the repository specifically for your submitted content, and in the comments for that PR identify
you as the submitter. We'll also link the PR to that Issue number, and the reStructuredText source code will contain a reST
comment (not visible in the web page) as mentioned above under `Submitting through email or Twitter`_.

Creating a GitHub pull request
------------------------------
If you really want control over the change, have a large number of changes to submit, or just want your GitHub user name 
in the commit history of the project, a GitHub pull request is the way to go. Assuming you're logged in to GitHub, once you 
find the page that should be edited from the search you conducted above (the page holding the full def for the flag, see above):

1. Click on the "Edit on GitHub" button in the upper right of the page. This takes you to GitHub, specifically to the
   reStructuredText (.rst) file that corresponds to the web page you just left. Make a note of which file you need
   to edit.
2. Click on the "Fork" button in the upper right.
3. In your forked repository, in the **master** branch, make the changes to the .rst file you noted above.
4. Submit a `pull request`_.

Feel free to add a reST comment (only visible in the source file) just above the flag you're adding/modifying with identifying info, like so::

	.. Submitted by: (Twitter) @FlagSubmitter

We'll review your PR, discuss back-and-forth if any clarifications are needed, and then merge in to the master branch
on our side.

.. note::

    This is a simple project, so you'll almost always make your changes to the master branch in your fork.
    A more complicated series of changes or re-working of the structure may warrant the creation of a new branch.
	Communicate with us (preferably via an issue) if you think this situation applies.
		
.. warning::
	
	Never make your changes or submit your pull request for a branch beginning with "DONOTUSE". The current
	maintainer uses these as private feature branches and pushes to them from his laptop regularly to 
	save WIP, then rebases later to clean up history before merging to master.


   
Guidelines for changes
----------------------

1. Normally, every flag should have one or more links to publicly-available content.

  A. If you are a Microsoft employee with source code access or a well-known figure in the SQL Server community with a reputation
     for accurate, detailed internals information (e.g. Paul White, Paul Randal, Dima Piliugin, etc) and you want to provide 
     never-before-seen info on new or existing flags, we welcome that even if there's no public link.
  B. Citations from books (e.g. the SQL Server Internals series) are fine as well, as long as an exact quote and a page number 
     is provided. 
  C. If you *aren't* a "1.A" type of person, but have a blog post with examples of new flags or new behavior for existing flags, 
     the flag w/that link is certainly welcome! We just may caveat the flag in some way to highlight the uncertainty around the 
     its functionality.
  
2. For existing flags, new content should add something not already obvious from the existing content. (This repository is not attempting 
   to document every blog post that uses Trace Flag 1117!) If a link has an unusual usage of the flag, an interesting side-effect, or some 
   other novel addition to the flag's current content in the repository, then it will certainly be welcome.

3. A link to a KB article or a blog by someone at Microsoft, or a "super-guru", is more valuable than a forum conversation or a post 
   by someone at the fringes of the SQL Server community. Of course, if their post adds value in some other way (creative use, interesting 
   side effects, etc) then it should be included along with any KB article or Microsoft blog post. And if the only link for a flag is to a 
   forum post or an unknown blogger, the repository is richer for it. (There just may need to be a caveat about the uncertainy.)

Any questions? Contact us through Twitter_, or send us an email! (aaron no-intervening-characters morelli at zoho daught com).
We look forward to your content!


.. Links

.. _GitHub: https://github.com/AaronMorelli/TopicalTraceFlags

.. _search: https://readthedocs.org/projects/topicaltraceflags/search/

.. _Twitter: https://twitter.com/sqlcrossjoin

.. _fork: https://help.github.com/articles/fork-a-repo/

.. _pull request: https://help.github.com/articles/creating-a-pull-request/

.. _submitting an issue: https://help.github.com/articles/creating-an-issue/

