===================
Topical Trace Flags
===================

*Purpose*: A collection of SQL Server trace flags organized by area of the SQL Server engine.
 
*Editor/Primary compiler*: Aaron Morelli
  
*Short Disclaimer*: The majority of these flags are undocumented by Microsoft, and therefore unsupported. The author has experimented 
with only a handful of these flags and cannot vouch for complete accuracy in the descriptions provided, nor has authority to do so. 
The author assumes no responsibility for any negative (or positive!) consequences arising from the use or misuse of these 
trace flags in any production or non-production environment. Use at your own risk.

*How to use*: Disclaimers_ | Conventions_ | License_ | Search_ | `Misc TF Usage Notes`_ | `Other Notable Repositories`_ | `Change Log`_

Categories
----------

**Queries**
  #. `Statistics and Estimation`_
  #. Compilation (Informational)
  #. Compilation (Behavioral)
  #. Plan Caching
  #. Query Execution

**Databases**
  #. Databases, Database Files, and Allocation (incl TempDB)
  #. Locking and Waiting
  #. Transactions, Logging, and Recovery
  #. Backup and Restore
  #. CHECKDB and Corruptions

**Server**
  #. Instance Startup, Database Startup, and General Instance Behavior
  #. SQL Miscellaneous Informational (especially re: Error Log)
  #. SQLOS Scheduling and CPU-related
  #. SQLOS Memory and Buffer Pool
  #. Disk and Network IO
  #. Connectivity
  #. Background Sessions and Tasks
  #. High Availability/Distributed Technologies
  #. Security
  #. Special Features
  #. Exceptions and Memory Dump Behavior
  
**Other**
  #. Other
  #. Mis-labeled, unable to find links, unable to confirm
  #. To-Do List
  
 
.. Links
.. _Disclaimers: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/Disclaimers.rst
.. _Conventions: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/Conventions.rst
.. _License: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/LICENSE
.. _Search: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/Search.rst
.. _Misc TF Usage Notes: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/MiscTFUsageNotes.rst
.. _Other Notable Repositories: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/OtherRepos.rst
.. _Change Log: https://github.com/AaronMorelli/TopicalTraceFlags/blob/master/core/ChangeLog.rst

.. _Statistics and Estimation: http://www.python.org/
