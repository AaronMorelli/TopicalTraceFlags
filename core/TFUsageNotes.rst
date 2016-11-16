======================
Trace Flag Usage Notes
======================

Most (but not all) trace flags can be enabled at SQL Server startup by using the –T (capital letter) startup option. 
However, most (but not all) trace flags can also be enabled at startup by using the –t (lowercase letter) startup option. 
(Besides the upper/lowercase difference, the syntax is otherwise identical). However, BOL_ indicates that other, unknown 
trace flags are enabled by the lowercase option, and thus the uppercase method should be used except under the direction 
of Microsoft support.

`Be careful`_ when adding Trace Flags (or other startup options) to the SQL Server via the Configuration Manager GUI.

Microsoft employee Nacho Alonso Portillo notes_ the following about the DBCC TRACEON command:

- The valid range for trace flags for 2008 R2 and before is -1 to 9299. Note that "9299" as an upper limit is clearly dated, as there are now trace flags with #’s higher than 9299, such as the well-known 9481, and apparently even a 10202 in “vNext” (the version after SQL 2014).
- However, "-1" has the well-known special meaning of enabling/disabling the flag globally. 
- The "-1" wildcard can appear in any position in the DBCC TRACEON syntax, though by convention most online use of it 
  always pushes it to the back of the command, after other TF #’s.
- Some flags can only be used with the "-T" startup option, and some can only be used with the DBCC TRACEON command. 
  Attempting to enable "-T"-only flags with DBCC TRACEON will result in an error.

Trace flags that affect query plan or query execution behavior can be enabled for individual queries 
(rather than session- or system-wide) via the QUERYTRACEON query hint. Not all trace flags are officially 
supported with QUERYTRACEON; KB2801413_ documents those that are.

KB974006_ notes: “If DBCC TRACEON\TRACEOFF is used this does not regenerate a new cached plan for stored procedures. 
Plans could be in cache that were created without the trace flag”

TF 4199 (officially described in KB974006_) is a special flag:

- Starting with SQL 2000 SP3, the query optimizer team has released all hotfix code under a policy such that the fix remains disabled until TF 4199 is enabled, unless that fix corrects a bug related to incorrect results or assertions or the like. 
- SQL Sasquatch (`Blog <http://sql-sasquatch.blogspot.com/>`_ | `Twitter <https://twitter.com/sql_handle>`_) has a short blog series on TF 4199, in which he notes that there are `risks inherent <http://sql-sasquatch.blogspot.com/2014/01/trace-flag-4199-complex-risk-assessment.html>`_ in enabling TF 4199 system-wide. This makes Microsoft’s official support for the QUERYTRACEON individual query hint even more important and useful.
- In the same blog series, SQL Sasquatch `points out <http://sql-sasquatch.blogspot.com/2014/01/trace-flag-4199-complex-risk-assessment_6.html>`_ that Microsoft has released several fixes recently that seem to violate the TF 4199 policy described above.


A 5-year-old `blog post <http://sqlblog.com/blogs/andrew_kelly/archive/2009/06/21/trace-flag-groupings.aspx>`_ by Andrew Kelly relays some information from Paul Randal about how trace flags are categorized. This provides some level of insight into how Microsoft categorizes (or perhaps categorized at some point in the past) their trace flags, though even a cursory glance through the lists below show that many flags do not fall into any of the below categories: 

- 25xx, 52xx are DBCC related 
- 8xx are buffer pool 
- 36xx are SQL Server general 'run-time' 
- 6xx are Storage Engine access methods 
- 12xx are lock manager 
- 14xx are database mirroring 
- 30xx, 31xx, 32xx are backup/restore 
- 55xx are FILESTREAM 
- 73xx, 74xx are query execution 
- 75xx are cursors 
- 82xx are replication

.. Links 
.. _BOL: https://msdn.microsoft.com/en-us/library/ms190737.aspx
.. _Be careful: http://blogs.msdn.com/b/psssql/archive/2010/02/19/did-you-start-your-sql-server-engine-correctly.aspx
.. _notes: http://blogs.msdn.com/b/ialonso/archive/2011/12/05/what-is-the-expected-behavior-from-an-attempt-to-enable-a-trace-flag-which-is-not-defined-in-the-targeted-version-of-the-product.aspx
.. _KB2801413: http://support.microsoft.com/kb/2801413/en-us
.. _KB974006: http://support.microsoft.com/kb/974006/en-us
.. _Blog: 
